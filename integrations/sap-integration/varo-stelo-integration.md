# Varo/Stelo Integration

## Triggering Looply from Varo/Stelo

You can trigger a Looply workflow as part of your Varo or Stelo process by calling method **/LOOPLY/CORE=>TRIGGER\_WF** as described [here](triggering-or-resuming-a-looply-workflow-from-sap.md).

As part of the trigger, you may wish to pass the form/app data to Looply as a JSON string. Varo users on version 320 or later can use function module /FLM/GET\_DOC\_DATA\_JSON to retrieve it. Users on earlier versions can create a copy of the function module in the Z-namespace as follows:&#x20;

Create function group ZFLM\_GET\_DOC\_DATA\_JSON with the following master program:

```abap
*******************************************************************
*   System-defined Include-files.                                 *
*******************************************************************
  INCLUDE lzflm_get_doc_data_jsontop.        " Global Declarations
  INCLUDE lzflm_get_doc_data_jsonuxx.        " Function Modules

*******************************************************************
*   User-defined Include-files (if necessary).                    *
*******************************************************************
  INCLUDE lzflm_get_doc_data_jsonsub.        " Subroutines
```

#### Function group include LZFLM\_GET\_DOC\_DATA\_JSONTOP:

```abap
FUNCTION-POOL zflm_get_doc_data_json.       "MESSAGE-ID ..

* INCLUDE LZFLM_GET_DOC_DATA_JSOND...        " Local class definition

TYPES: BEGIN OF gtyp_repeating_sf,
         subform TYPE /flm/sfs_sf,
       END OF gtyp_repeating_sf.
*
DATA: gs_fpe          TYPE /flm/fpe,
      gt_fdata        TYPE /flm/xml_tab_t,
      gt_repeating_sf TYPE TABLE OF gtyp_repeating_sf,
      gv_dd_text      TYPE flag.
```



#### Function group include LZFLM\_GET\_DOC\_DATA\_JSONSUB

```abap
*----------------------------------------------------------------------*
***INCLUDE LZFLM_GET_DOC_DATA_JSONSUB.
*----------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*& Form get_json_data
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*&      --> P_SF
*&      --> P_REPEATING
*&      <-- P_JSON_DATA
*&---------------------------------------------------------------------*
FORM get_json_data  USING    p_sf        TYPE string
                             p_repeating TYPE flag
                    CHANGING p_json_data TYPE string.
*
  DATA: lv_row_num      TYPE numc3,
        lv_path         TYPE string,
        lv_length       TYPE i,
        ls_fdata        TYPE /flm/xml_tab,
        ls_fdata2       TYPE /flm/xml_tab,
        ls_repeating_sf TYPE gtyp_repeating_sf,
        lv_child_num    TYPE i.
*
  IF p_repeating IS INITIAL.
*
* Non repeating subform
*
    CONCATENATE '"' p_sf '":{' INTO p_json_data.
*
* Loop around children
*
    lv_child_num = 0.
    LOOP AT gt_fdata INTO ls_fdata WHERE parent EQ p_sf.
*
      lv_child_num = lv_child_num + 1.
*
      PERFORM process_sf_child USING ls_fdata
                                     lv_child_num
                            CHANGING p_json_data.
*
    ENDLOOP.
*
    CONCATENATE: p_json_data '}' INTO p_json_data.
*
  ELSE.
*
* Repeating subform
*
    READ TABLE gt_fdata INTO ls_fdata WITH KEY name = p_sf.
    ls_repeating_sf-subform = ls_fdata-name.
    APPEND ls_repeating_sf TO gt_repeating_sf. "Save the fact that we've processed this repeating sf
    CONCATENATE '"' p_sf '":[' INTO p_json_data.
    CLEAR lv_row_num.
*
* Process one row in each loop
*
    DO.
*
      lv_row_num = lv_row_num + 1.
      lv_path = ls_fdata-path.
      lv_length = strlen( lv_path ).
      lv_length = lv_length - 3.
      CONCATENATE: lv_path+0(lv_length) lv_row_num INTO lv_path.
      READ TABLE gt_fdata WITH KEY path = lv_path TRANSPORTING NO FIELDS.
      IF sy-subrc IS NOT INITIAL.
        EXIT.
      ENDIF.
*
      IF lv_row_num EQ '001'.
        CONCATENATE: p_json_data '{' INTO p_json_data.
      ELSE.
        CONCATENATE: p_json_data ',{' INTO p_json_data.
      ENDIF.
      lv_child_num = 0.
*
      LOOP AT gt_fdata INTO ls_fdata2 WHERE parent = ls_fdata-name AND path CS lv_path.
*
        lv_child_num = lv_child_num + 1.
*
        PERFORM process_sf_child USING ls_fdata2
                                       lv_child_num
                              CHANGING p_json_data.
*
      ENDLOOP.
*
      CONCATENATE: p_json_data '}' INTO p_json_data.
*
    ENDDO.
*
    CONCATENATE: p_json_data ']' INTO p_json_data.
*
  ENDIF.
*
ENDFORM.
*&---------------------------------------------------------------------*
*& Form process_sf_child
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*&      --> P_FDATA
*&      --> P_CHILD_NUM
*&      <-- P_JSON_DATA
*&---------------------------------------------------------------------*
FORM process_sf_child  USING    p_fdata     TYPE /flm/xml_tab
                                p_child_num TYPE i
                       CHANGING p_json_data TYPE string.

  DATA: lv_maxoccurs      TYPE /flm/sfs_sf_max,
        lv_json_data_temp TYPE string,
        lv_repeating      TYPE flag,
        lv_separator(1)   TYPE c,
        lv_fld_value      TYPE string.
*
  IF p_child_num GT 1.
    lv_separator = ','.
  ELSE.
    CLEAR lv_separator.
  ENDIF.
*
  SELECT SINGLE maxoccurs FROM /flm/fdd_sf INTO lv_maxoccurs
    WHERE cust_code EQ gs_fpe-ccode
     AND  ftype     EQ gs_fpe-ftype
     AND  flang     EQ gs_fpe-flang
     AND  fver      EQ gs_fpe-fver
     AND  subform   EQ p_fdata-name.
*
  IF sy-subrc IS INITIAL.
*
* Child subform
*
    READ TABLE gt_repeating_sf WITH KEY subform = p_fdata-name TRANSPORTING NO FIELDS.
    IF sy-subrc IS INITIAL.
*       This is a repeating sf we've allready processed but because it is repeating it appears more than once in gt_fdata. Don't process again
      RETURN.
    ENDIF.
*
    IF lv_maxoccurs GT '0001'.
      lv_repeating = 'X'.
    ELSE.
      CLEAR lv_repeating.
    ENDIF.
*
    PERFORM get_json_data USING    p_fdata-name
                                   lv_repeating
                          CHANGING lv_json_data_temp.
*
    CONCATENATE p_json_data lv_separator lv_json_data_temp INTO p_json_data.
*
  ELSE.
*
* Child field
*
    lv_fld_value = p_fdata-value.
    IF gv_dd_text IS NOT INITIAL.
      PERFORM format_fld_value USING p_fdata-name
                               CHANGING lv_fld_value.
    ENDIF.
    PERFORM escape_json CHANGING lv_fld_value.
    CONCATENATE: p_json_data lv_separator '"' p_fdata-name '":"' lv_fld_value '"' INTO p_json_data.
*
  ENDIF.
*
ENDFORM.
*&---------------------------------------------------------------------*
*& Form format_fld_value
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*&      --> P_FDATA_NAME
*&      <-- P_FLD_VALUE
*&---------------------------------------------------------------------*
FORM format_fld_value  USING    p_fld_name
                       CHANGING p_fld_value.
*
  DATA: lv_fld_type TYPE /flm/sfs_field_type,
        lt_f4_data  TYPE /flm/sfs_form_data_t,
        ls_f4_data  TYPE /flm/form_data.
*
  SELECT SINGLE field_type FROM /flm/fdd_fld INTO lv_fld_type
    WHERE ccode      EQ gs_fpe-ccode
     AND  ftype      EQ gs_fpe-ftype
     AND  flang      EQ gs_fpe-flang
     AND  fver       EQ gs_fpe-fver
     AND  field_name EQ p_fld_name.
*
  IF sy-subrc IS INITIAL AND lv_fld_type EQ 'DROP'.
*
    CALL METHOD /flm/hds=>get_f4_entries_for_field
      EXPORTING
        im_ccode      = gs_fpe-ccode
        im_field_name = p_fld_name
        im_fstatus    = gs_fpe-fstatus
        im_flang      = gs_fpe-flang
        im_user       = sy-uname
        im_fver       = gs_fpe-fver
        im_ftype      = gs_fpe-ftype
        im_doc        = gs_fpe-document
        im_form_data  = gt_fdata
        im_prev_page  = ''
        im_excel      = 'X'
      IMPORTING
        ex_f4_data    = lt_f4_data.
*
    READ TABLE lt_f4_data INTO ls_f4_data WITH KEY name = p_fld_value.
    IF sy-subrc IS INITIAL.
      p_fld_value = ls_f4_data-value.
    ENDIF.
*
  ENDIF.
*
ENDFORM.
*&---------------------------------------------------------------------*
*& Form escape_json
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*&      <-- LV_FLD_VALUE
*&---------------------------------------------------------------------*
FORM escape_json  CHANGING p_fld_value.
*
  REPLACE ALL OCCURRENCES OF `\` IN p_fld_value WITH `\\`.
  REPLACE ALL OCCURRENCES OF `"` IN p_fld_value WITH `\"`.
  REPLACE ALL OCCURRENCES OF cl_abap_char_utilities=>cr_lf          IN p_fld_value WITH `\n\n`.  "use \n\n instead of the standard \r\n here as teams cards don't understand \r\n
  REPLACE ALL OCCURRENCES OF cl_abap_char_utilities=>newline        IN p_fld_value WITH `\n\n`.  "use \n\n instead of the standard \n here as teams cards don't understand. \n\n creates two newlines in a card where idealy we'd only want one
  REPLACE ALL OCCURRENCES OF cl_abap_char_utilities=>horizontal_tab IN p_fld_value WITH `\t`.
*
ENDFORM.
```



Finally, create a new function module and add it to the function group:

#### Function module ZFLM\_GET\_DOC\_DATA\_JSON

```abap
FUNCTION zflm_get_doc_data_json.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(IM_CCODE) TYPE  /FLM/CUST_CODE
*"     VALUE(IM_FTYPE) TYPE  /FLM/FTYPE_CODE
*"     VALUE(IM_FLANG) TYPE  /FLM/FLANG
*"     VALUE(IM_FVER) TYPE  /FLM/FVER
*"     VALUE(IM_ID) TYPE  /FLM/FID
*"     VALUE(IM_ID_VAR) TYPE  /FLM/ID_VAR
*"     VALUE(IM_DD_TEXT) TYPE  FLAG OPTIONAL
*"     VALUE(IM_FORM_DATA) TYPE  /FLM/XML_TAB_T OPTIONAL
*"  EXPORTING
*"     VALUE(EX_JSON_DATA) TYPE  STRING
*"----------------------------------------------------------------------
*
*-------------------------------------------------------------------------------------------------------------------------------------------*
* This function module can be used to retrieve the data from your FLM form or Stelo app in JSON format
* Setting the IM_DD_TEXT parameter to 'X' will cause the function to return the text rather than the key for drop-down fields
* If import parameter IM_FORM_DATA is left blank, the function will read the data from the content server. If data is passed in using this
* import parameter, that data will be converted isntead of the CMS data. This could be useful if you wish to format or change the data before
* it is converted to JSON
*-------------------------------------------------------------------------------------------------------------------------------------------*
*
  CLEAR: gs_fpe, gt_fdata, gt_repeating_sf.
  gv_dd_text = im_dd_text.
*
  SELECT SINGLE * INTO gs_fpe FROM /flm/fpe
    WHERE ccode  EQ im_ccode
     AND  ftype  EQ im_ftype
     AND  flang  EQ im_flang
     AND  fver   EQ im_fver
     AND  id     EQ im_id
     AND  id_var EQ im_id_var.
*
  IF im_form_data IS NOT INITIAL.
    gt_fdata = im_form_data.
  ELSE.
*
    CALL METHOD /flm/core=>get_data_from_instance
      EXPORTING
        im_form_instance = gs_fpe
      RECEIVING
        ex_form_data     = gt_fdata.
*
  ENDIF.
*
  PERFORM get_json_data USING    'DATA'
                                 ''
                        CHANGING ex_json_data.
*
  CONCATENATE: '{' ex_json_data '}' INTO ex_json_data.
*
ENDFUNCTION.
*
```



## Approving a Varo form/Stelo document from a Looply card

In order to approve a Varo form or Stelo document from a Looply card you will need to make a POST request to SAP from your Looply workfow as described [here](triggering-sap-code-from-looply.md#post-data-to-sap). In your function, you can use function module /FLM/DOCUMENT\_PROCESS\_ACTION to process the approval or rejection. If you wish to also update the form/app data and are using Varo version 310 or earlier you can install [Varo note 254](http://support.arch-global.com/default.asp?W206) . This adds new import parameter IM\_FORM\_DATA to the function module for the new/updated data.



## Example

The following is an example of a simple use-case: when a user submits a Varo form or a Stelo app, an approver gets a Teams card with information about the request, an input field for adding comments and options to approve or reject the request. The approver action (and any added comments) are processed by the Varo  back-end. If the request is approved, the process is complete. If it is rejected, the user is notified in Teams and has the option to re-submit or cancel the request.

In the Looply _Process Determination_ table, we have the following configuration:

<figure><img src="../../.gitbook/assets/Process Determination.png" alt=""><figcaption></figcaption></figure>

The workflow is triggered and resumed in the _routing user-exit_ (you may also use posting adaptors to do this) depending on the routing step/FLM action:

```abap
METHOD wf_stw3 .
*
  TYPES: BEGIN OF ltyp_util_data,
           cms_doc   TYPE /flm/cms_doc,
           approver  TYPE string,
           initiator TYPE string,
         END OF ltyp_util_data.
*
  DATA: lv_process_id    TYPE /looply/process_id,
        lv_subrc         TYPE sysubrc,
        ls_mess          TYPE bapiret2,
        ls_util_data     TYPE ltyp_util_data,
        lv_util_data     TYPE string,
        lv_data          TYPE string,
        ls_fpe           TYPE /flm/fpe,
        ls_address       TYPE bapiaddr3,
        lt_return        TYPE TABLE OF bapiret2,
        ls_activity      TYPE /looply/activity,
        lv_looply_action TYPE string,
        lt_callstack     TYPE sys_callst.
*
  CONCATENATE: sy-sysid im_instance-ccode im_instance-ftype im_instance-id INTO lv_process_id SEPARATED BY '-'. "Create unique process id
  lv_looply_action = im_action.
*
* Read Looply activity table
  SELECT SINGLE * FROM /looply/activity INTO ls_activity
    WHERE scenario         EQ /looply/core=>c_scenario_vo
     AND  scenario_id      EQ im_instance-ftype
     AND  scenario_version EQ im_instance-fver
     AND  step             EQ '001'.
*
  IF im_action EQ 'S'.
*
    ex_owner = 'USER2'. "Harcoded for demonstration purposes
*
    CONCATENATE: im_instance-ccode im_instance-ftype im_instance-flang im_instance-fver im_instance-id im_instance-id_var INTO ls_util_data-cms_doc SEPARATED BY '-'.
* Get approver and initiator email address
    CALL FUNCTION 'BAPI_USER_GET_DETAIL'
      EXPORTING
        username = im_instance-finitiator
      IMPORTING
        address  = ls_address
      TABLES
        return   = lt_return.
*
    ls_util_data-initiator = ls_address-e_mail.
    CLEAR: ls_address, lt_return.
*
    CALL FUNCTION 'BAPI_USER_GET_DETAIL'
      EXPORTING
        username = ex_owner
      IMPORTING
        address  = ls_address
      TABLES
        return   = lt_return.
*
    ls_util_data-approver = ls_address-e_mail.
    lv_util_data = /ui2/cl_json=>serialize( data = ls_util_data pretty_name = /ui2/cl_json=>pretty_mode-low_case ).
*
* Get document data in json format
    CALL FUNCTION 'ZFLM_GET_DOC_DATA_JSON'
      EXPORTING
        im_ccode     = im_instance-ccode
        im_ftype     = im_instance-ftype
        im_flang     = im_instance-flang
        im_fver      = im_instance-fver
        im_id        = im_instance-id
        im_id_var    = im_instance-id_var
        im_dd_text   = 'X'
      IMPORTING
        ex_json_data = lv_data.
*
* Check whether we are re-submitting a rejected document or submitting a new one.
    SELECT SINGLE * FROM /flm/fpe INTO ls_fpe
      WHERE ccode   EQ im_instance-ccode
       AND  ftype   EQ im_instance-ftype
       AND  flang   EQ im_instance-flang
       AND  fver    EQ im_instance-fver
       AND  id      EQ im_instance-id
       AND  fstatus EQ 'R'.
*
    IF sy-subrc IS NOT INITIAL. "We're submitting a new form so trigger Looply wf.
*
      CALL METHOD /looply/core=>trigger_wf
        EXPORTING
          im_looply_wf         = ls_activity-looply_wf
          im_looply_wf_version = ls_activity-looply_wf_version
          im_scenario          = ls_activity-scenario
          im_scenario_id       = ls_activity-scenario_id
          im_scenario_version  = ls_activity-scenario_version
          im_step              = ls_activity-step
          im_process_id        = lv_process_id
          im_data              = lv_data
          im_util_data         = lv_util_data
        IMPORTING
          ex_subrc             = lv_subrc
          ex_mess              = ls_mess.
*
    ELSE. "Initiator is re-submitting a rejected form, so resume Looply wf
*
      CALL METHOD /looply/core=>resume_wf
        EXPORTING
          im_process_id       = lv_process_id
          im_data             = lv_data
          im_util_data        = lv_util_data
          im_action           = lv_looply_action
          im_scenario         = /looply/core=>c_scenario_vr
          im_scenario_id      = ls_activity-scenario_id
          im_scenario_version = ls_activity-scenario_version
          im_step             = ls_activity-step
        IMPORTING
          ex_subrc            = lv_subrc
          ex_mess             = ls_mess.
*
    ENDIF.
*
  ELSEIF im_action EQ 'X'. "Initiator has cancelled a rejected form
*
    ex_owner = 'FLM_USER'.
*
    CALL METHOD /looply/core=>resume_wf
      EXPORTING
        im_process_id       = lv_process_id
        im_action           = lv_looply_action
        im_scenario         = /looply/core=>c_scenario_vr
        im_scenario_id      = ls_activity-scenario_id
        im_scenario_version = ls_activity-scenario_version
        im_step             = ls_activity-step
      IMPORTING
        ex_subrc            = lv_subrc
        ex_mess             = ls_mess.
*
  ELSEIF im_action EQ 'A' OR im_action EQ 'R'. "Approver has approved or rejected
*
    IF im_action EQ 'A'.
      ex_owner = 'FLM_USER'.
    ELSE.
      ex_owner = im_instance-finitiator.
    ENDIF.
*
* If action happened via Fiori launchpad, resume the WF
*
    CALL FUNCTION 'SYSTEM_CALLSTACK'
      IMPORTING
        et_callstack = lt_callstack.
*
    READ TABLE lt_callstack WITH KEY progname = '/STELO/CL_APP_SERVER_DPC_EXT==CP' TRANSPORTING NO FIELDS.
    IF sy-subrc IS INITIAL.
*
      CALL METHOD /looply/core=>resume_wf
        EXPORTING
          im_process_id       = lv_process_id
          im_action           = lv_looply_action
          im_scenario         = /looply/core=>c_scenario_vr
          im_scenario_id      = ls_activity-scenario_id
          im_scenario_version = ls_activity-scenario_version
          im_step             = ls_activity-step
        IMPORTING
          ex_subrc            = lv_subrc
          ex_mess             = ls_mess.
*
    ENDIF.
*
  ENDIF.
*
ENDMETHOD.
```



The above code triggers the following workflow:

{% @arcade/embed flowId="UllRBLy6R699Zb0Nd8eI" url="https://app.arcade.software/share/UllRBLy6R699Zb0Nd8eI" %}



In the approver card we have the following section, containing the comments input field, action buttons and error message:

```json
{
    "type": "Container",
    "items": [
        {
            "id": "comment",
            "placeholder": "Approver Comment",
            "type": "Input.Text",
            "isMultiline": true
        },
        {
            "type": "ColumnSet",
            "columns": [
                {
                    "type": "Column",
                    "width": "stretch",
                    "items": [
                        {
                            "type": "ActionSet",
                            "actions": [
                                {
                                    "style": "positive",
                                    "type": "Action.Submit",
                                    "title": "Approve",
                                    "data": {
                                        "action": "A",
                                        "comment": ""
                                    }
                                },
                                {
                                    "type": "Action.Submit",
                                    "title": "Reject",
                                    "data": {
                                        "action": "R",
                                        "comment": ""
                                    }
                                }
                            ],
                            "horizontalAlignment": "Right"
                        }
                    ]
                }
            ]
        },
        {
            "style": "attention",
            "type": "Container",
            "$when": "${$root.payload.output.show_error}",
            "bleed": true,
            "items": [
                {
                    "weight": "Bolder",
                    "horizontalAlignment": "Center",
                    "text": "${$root.function_2.output}",
                    "type": "TextBlock",
                    "wrap": true
                }
            ]
        }
    ],
    "spacing": "ExtraLarge"
}
```



When the approver clicks on an action button on the card, the following Z-function is triggered:

```abap
FUNCTION zlooply_stw3_approve.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(IM_DATA) TYPE  STRING
*"  EXPORTING
*"     VALUE(EX_SUBRC) TYPE  SYSUBRC
*"     VALUE(EX_MESS) TYPE  BAPIRET2
*"     VALUE(EX_DATA) TYPE  STRING
*"----------------------------------------------------------------------
  TYPES: BEGIN OF ltyp_data,
           cms_doc TYPE /flm/cms_doc,
           action  TYPE /flm/faction,
           comment TYPE string,
         END OF ltyp_data.
*
  DATA: ls_data        TYPE ltyp_data,
        lv_ccode       TYPE /flm/cust_code,
        lv_ftype       TYPE /flm/ftype_code,
        lv_flang       TYPE /flm/flang,
        lv_fver        TYPE /flm/fver,
        lv_fid         TYPE /flm/fid,
        lv_fid_var     TYPE /flm/id_var,
        ls_fpe         TYPE /flm/fpe,
        lt_form_data   TYPE /flm/xml_tab_t,
        ls_form_data   TYPE /flm/xml_tab,
        lt_row         TYPE TABLE OF string,
        lv_row         TYPE string,
        lv_row2        TYPE string,
        lv_length      TYPE i,
        lv_row_num     TYPE numc3,
        lv_row_max     TYPE numc3,
        lv_row_plus    TYPE numc3,
        lv_path        TYPE string,
        lv_path2       TYPE string,
        lv_path_max    TYPE string,
        lv_path_plus   TYPE string,
        ls_address     TYPE bapiaddr3,
        lt_return      TYPE TABLE OF bapiret2,
        lv_date        TYPE string,
        lv_day         TYPE string,
        lv_time        TYPE string,
        lv_hrs         TYPE numc2,
        lv_fstat_name  TYPE /flm/fstatus_name,
        lt_month_names TYPE TABLE OF T247,
        ls_month_names TYPE T247.
*
* Get data from card
*
  /ui2/cl_json=>deserialize( EXPORTING json = im_data CHANGING data = ls_data ).
*
  CALL METHOD /flm/core=>split_xdp_cms_doc
    EXPORTING
      im_cms_doc = ls_data-cms_doc
    IMPORTING
      ex_ccode   = lv_ccode
      ex_ftype   = lv_ftype
      ex_flang   = lv_flang
      ex_fver    = lv_fver
      ex_fid     = lv_fid
      ex_fid_var = lv_fid_var.
*
  SELECT SINGLE * FROM /flm/fpe INTO ls_fpe
    WHERE ccode  EQ lv_ccode
     AND  ftype  EQ lv_ftype
     AND  flang  EQ lv_flang
     AND  fver   EQ lv_fver
     AND  id     EQ lv_fid
     AND  id_var EQ lv_fid_var.
*
* Update data with approver comment (if there is one)
*
  IF ls_data-comment IS NOT INITIAL.
*
    lv_day = sy-datum+6(2).
    SHIFT lv_day LEFT DELETING LEADING '0'.
*
    CALL FUNCTION 'MONTH_NAMES_GET'
      TABLES
        month_names = lt_month_names.
*
    READ TABLE lt_month_names INTO ls_month_names WITH KEY mnr = sy-datum+4(2).
    CONCATENATE ls_month_names-LTX lv_day INTO lv_date SEPARATED BY space.
    CONCATENATE lv_date ',' INTO lv_date.
    CONCATENATE lv_date sy-datum+0(4) INTO lv_date SEPARATED BY space.
*
    lv_hrs = sy-uzeit+0(2).
    IF lv_hrs GT '12'.
      lv_hrs = lv_hrs - 12.
      CONCATENATE: lv_hrs ':' sy-uzeit+2(2) ' PM' INTO lv_time.
    ELSE.
      CONCATENATE: lv_hrs ':' sy-uzeit+2(2) ' AM' INTO lv_time.
    ENDIF.
    SHIFT lv_time LEFT DELETING LEADING '0'.
    CONCATENATE lv_date 'at' lv_time INTO lv_date SEPARATED BY space.
*
    SELECT SINGLE fstat_name FROM /flm/fstatt INTO lv_fstat_name
      WHERE spras EQ 'E'
       AND  ccode EQ ls_fpe-ccode
       AND  fstat EQ ls_fpe-fstatus.
*
    CALL FUNCTION 'BAPI_USER_GET_DETAIL'
      EXPORTING
        username = sy-uname
      IMPORTING
        address  = ls_address
      TABLES
        return   = lt_return.
*
    CALL METHOD /flm/core=>get_data_from_instance
      EXPORTING
        im_form_instance = ls_fpe
      RECEIVING
        ex_form_data     = lt_form_data.
*
    LOOP AT lt_form_data INTO ls_form_data WHERE name = 'SF_CMT'.
*
      SPLIT ls_form_data-path AT '.' INTO TABLE lt_row.
      DESCRIBE TABLE lt_row LINES lv_length.
      READ TABLE lt_row INDEX lv_length INTO lv_row.
      lv_row_num = lv_row.
      IF lv_row_num GT lv_row_max.
        lv_row_max = lv_row_num.
        lv_path_max = ls_form_data-path.
      ENDIF.
*
    ENDLOOP.
*
    READ TABLE lt_form_data INTO ls_form_data WITH KEY name = 'CMT_TEXT'.
    IF lv_row_max = '001' AND ls_form_data-value IS INITIAL. "ie no existing comments so modify emptyy row
*
      READ TABLE lt_form_data INTO ls_form_data WITH KEY name = 'CMT_DATE'.
      ls_form_data-value = lv_date.
      MODIFY lt_form_data INDEX sy-tabix FROM ls_form_data.
*
      READ TABLE lt_form_data INTO ls_form_data WITH KEY name = 'CMT_STATUS'.
      ls_form_data-value = lv_fstat_name.
      MODIFY lt_form_data INDEX sy-tabix FROM ls_form_data.
*
      READ TABLE lt_form_data INTO ls_form_data WITH KEY name = 'CMT_TEXT'.
      ls_form_data-value = ls_data-comment.
      MODIFY lt_form_data INDEX sy-tabix FROM ls_form_data.
*
      READ TABLE lt_form_data INTO ls_form_data WITH KEY name = 'CMT_USER_NAME'.
      ls_form_data-value = ls_address-fullname.
      MODIFY lt_form_data INDEX sy-tabix FROM ls_form_data.
*
    ELSE.
*
      lv_row = lv_row_max.
      lv_row_plus = lv_row_max + 1.
      lv_row2 = lv_row_plus.
      lv_path_plus = lv_path_max.
      CONCATENATE: 'SF_CMT' '.' lv_row INTO lv_path.
      CONCATENATE: 'SF_CMT' '.' lv_row2 INTO lv_path2.
      REPLACE lv_path IN lv_path_plus WITH lv_path2.
*
      READ TABLE lt_form_data INTO ls_form_data WITH KEY path = lv_path_max.
      ls_form_data-path = lv_path_plus.
      APPEND ls_form_data TO lt_form_data.
*
      CONCATENATE: lv_path_max '/CMT_DATE.001' INTO lv_path.
      READ TABLE lt_form_data INTO ls_form_data WITH KEY path = lv_path.
      CONCATENATE: lv_path_plus '/CMT_DATE.001' INTO ls_form_data-path.
      ls_form_data-value = lv_date.
      APPEND ls_form_data TO lt_form_data.
*
      CONCATENATE: lv_path_max '/CMT_STATUS.001' INTO lv_path.
      READ TABLE lt_form_data INTO ls_form_data WITH KEY path = lv_path.
      CONCATENATE: lv_path_plus '/CMT_STATUS.001' INTO ls_form_data-path.
      ls_form_data-value = lv_fstat_name.
      APPEND ls_form_data TO lt_form_data.
*
      CONCATENATE: lv_path_max '/CMT_TEXT.001' INTO lv_path.
      READ TABLE lt_form_data INTO ls_form_data WITH KEY path = lv_path.
      CONCATENATE: lv_path_plus '/CMT_TEXT.001' INTO ls_form_data-path.
      ls_form_data-value = ls_data-comment.
      APPEND ls_form_data TO lt_form_data.
*
      CONCATENATE: lv_path_max '/CMT_USER_NAME.001' INTO lv_path.
      READ TABLE lt_form_data INTO ls_form_data WITH KEY path = lv_path.
      CONCATENATE: lv_path_plus '/CMT_USER_NAME.001' INTO ls_form_data-path.
      ls_form_data-value = ls_address-fullname.
      APPEND ls_form_data TO lt_form_data.
*
    ENDIF.
*
  ENDIF.
*
* Process action
*
  CALL FUNCTION '/FLM/DOCUMENT_PROCESS_ACTION'
    EXPORTING
      im_fpe       = ls_fpe
      im_user      = sy-uname
      im_action    = ls_data-action
      im_form_data = lt_form_data
    IMPORTING
      ex_mess      = ex_mess.
*
  IF ex_mess-type EQ /flm/core=>c_mess_error.
    ex_subrc = 4.
  ENDIF.
*
ENDFUNCTION.
```
