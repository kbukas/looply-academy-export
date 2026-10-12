---
hidden: true
---

# Note 1

**Note Version:** 1\
**Created Date:** 29 Apr 2026\
**Looply Version Validity:** 110-120

{% hint style="info" %}
This correction note contains manual changes for a Looply ABAP repository object. Apply the changes in the sequence shown, then save, activate, and transport the affected object.
{% endhint %}

### Symptoms

You get a Looply error when actioning, for example approving or rejecting, a standard Fiori app.

### Cause and Pre-Requisites

Program error.

### Solution

Apply the following changes to the listed ABAP repository object.

{% hint style="warning" %}
Copy only the code inside the code blocks. Do not copy the section headings, labels, or explanatory text into the ABAP editor.
{% endhint %}

#### Object 1: Method /LOOPLY/BADI\_INBOX\_UPDT=>/IWWRK/IF\_WF\_WI\_BEFORE\_UPD\_IB\~BEFORE\_UPDATE

**Object Type:** Class Method\
**Object Name:** /LOOPLY/BADI\_INBOX\_UPDT\
**Method:** /IWWRK/IF\_WF\_WI\_BEFORE\_UPD\_IB\~BEFORE\_UPDATE\
**Transaction:** SE24

**Change 1.1**

Find block:

```abap
  APPEND ls_mess TO ct_return.
```

Replace it with:

```abap
*  APPEND ls_mess TO ct_return.                   "Note 1--
```

**Change 1.2**

Find block:

```abap
  APPEND LINES OF lt_mess TO ct_return.
```

Replace it with:

```abap
*  APPEND LINES OF lt_mess TO ct_return.          "Note 1--
```

### Final Steps

Save, activate and transport your changes through the landscape.
