**Cheat Sheet**
===

- [Cheat Sheet](#cheat-sheet)
- [NIPRGPT Queries](#niprgpt-queries)
  - [5 Bullet Points](#5-bullet-points)
  - [Maintenance Summary](#maintenance-summary)
- [UTIL\_AJAX](#util_ajax)
  - [Package](#package)
  - [Package Body](#package-body)
- [AJAX](#ajax)
- [XHR File Upload](#xhr-file-upload)
- [Form Submit](#form-submit)
- [DataTables](#datatables)
- [Exception Clause](#exception-clause)

# Grant Queries

## get all grants

``` sql
select 'grant '||privilege||' on '||owner||'.'||table_name||' to '||grantee|| case when grantable='YES' then ' with grant option;' else ';' end
from dba_tab_privs
where (grantee like 'OPS$%'
    or grantee in ('PUBLIC','APEX_PUBLIC_USER'))
    and owner = user
;
```

## replace all public grants with apex_public_user grants

``` sql
select 'revoke '||privilege||' on '||owner||'.'||table_name||' from PUBLIC;' || ' grant '||privilege||' on '||owner||'.'||table_name||' to APEX_PUBLIC_USER;'
from dba_tab_privs
where (grantee like 'OPS$%'
    or grantee in ('PUBLIC'))
    and owner = user
;
```

# Common Issues

## Javascript not Loading

Query `lgap_rec` for the user by CERT_NAME in ops$navsup_util and check to see if `FETCH_DATE` is at all recent. If not, delete the record and have the user try to hit the page again.

``` sql
select *
from ldap_rec
where CERT_NAME = 'CN'
;
```

# UTIL_AJAX

## Package

``` sql
procedure _get; -- add name of table
procedure _update( -- add name of table
  p_type in varchar2
);
```

## Package Body

``` sql
procedure _get -- add name of table
as
  cur SYS_REFCURSOR
begin
  apex_json.open_object();
  apex_json.write('data', cur);
  apex_json.write('status', 'success');
  apex_json.close_object();

exception when others then
  apex_json.open_object();
  apex_json.write('status', 'failed');
  apex_json.write('sqlerrm', sqlerrm);
  apex_json.close_object();

end;

procedure _update ( -- add name of table
  p_type in varchar2
) as
begin
  case p_type
  when 'add' then
    INSERT INTO table_name (
      col
    ) VALUES (
      value
    );
  when 'update' then
    UPDATE table_name
    SET col = val
    WHERE condition;
  when 'delete' then
    DELETE FROM table_name
    WHERE condition;
  end;

exception when others then
  apex_json.open_object();
  apex_json.write('status', 'failed');
  apex_json.write('sqlerrm', sqlerrm);
  apex_json.close_object();

end;
```

# AJAX

``` html
htp.p('
<!-- start ajax javascript -->
<script type="text/javascript">

  const get = () => {
    $.ajax({
      type: "GET" // change if needed
      , url: `` // ajax function
      , data: // add if needed or remove
      , beforeSend: () => {
        // completes before sending
      }
      , success: (result) => { // success: (data, status, xhr)
        if (result.status == "success") {
          // call to init function
        } else {
          // make sure Toast is imported and add what did not load
          showToast(
            "text-danger"
            , "Error"
            , `Failed to load. ${(result.sqlerrm) ? result.sqlerrm : ""}`
          )
        }
      }
      , error: (xhr, status, error) => {
        console.error(
          "AJAX Error:"
          , status
          , error
        )
        // make sure Toast is imported and add what did not load
        showToast(
          "text-danger"
          , "Error"
          , `Failed to load: ${status}, ${error}`
        )
      }
      , complete: (xhr, status) => {
        // Handle complete (success or error)
      }
    })
  }

  $(() => get())
</script>
<!-- end ajax javascript -->
');
```

# XHR File Upload

``` html
htp.p('
<!-- start xhr file upload javascript -->
<script type="text/javascript">

  let xhr = new XMLHttpRequest()
  xhr.open("POST", "/apps/navsup/file/upload/", true)
  xhr.responseType = "text"
  xhr.setRequestHeader("file_name", name)
  xhr.setRequestHeader("mime_type", file.type)
  xhr.setRequestHeader("file_size", file.size)
  xhr.onload = function() {
    if (this.status == 201) {
      $("#fpds_file_code").val(xhr.responseText)
      $("#fpds_upload_file_label").text(name)
      $("#fpds_upload_submit").prop("disabled", false)
    } else {
      showToast(
        "text-danger"
        , "Error!"
        , "File Upload NOT Successful. Please try again."
      )
    }
  }
  xhr.send(file)
</script>
<!-- end xhr file upload javascript -->
');
```

# Form Submit

``` html
htp.p('
<!-- start form submit javascript -->
<script type="text/javascript">

  const loadingButton = (text) => {
    let newText;

    if (text.endsWith("ing")) {
      newText = text;
    } else if (
      /[aeiouy]$/.test(text) &&
      !/(ee|ie)$/.test(text) &&
      !/^[aeiouy][^aeiouy]$/.test(text)
    ) {
      newText = text + text.slice(-1) + "ing";
    } else if (/[^aeiouy]e$/.test(text)) {
      newText = text.slice(0, -1) + "ing";
    } else {
      newText = text + "ing";
    }

    return `
      <div class="spinner-border spinner-border-sm" role="status"></div> ${newText}...
    `
  }

  const Upload = () => {
    e.preventDefault()
    if (e.currentTarget === null) return
    let submitBtnEl = $(e.currentTarget).find(`[type="submit"]`)
    let submitBtnText = submitBtnEl.text()
    $.ajax({
      type: "POST" // change if needed
      , url: `` // ajax function
      , data: $(e.currentTarget).serialize()
      , beforeSend: () => {
        // completes before sending
        submitBtnEl
          .prop("disabled", true)
          .empty()
          .append(
            loadingButton(submitBtnText)
          )
      }
      , success: (result) => { // success: (data, status, xhr)
        if (result.status == "success") {
          // call to init function
        } else {
          // make sure Toast is imported and add what did not load
          showToast(
            "text-danger"
            , "Error"
            , `Failed to load. ${(result.sqlerrm) ? result.sqlerrm : ""}`
          )
        }
      }
      , error: (xhr, status, error) => {
        console.error(
          "AJAX Error:"
          , status
          , error
        )
        // make sure Toast is imported and add what did not load
        showToast(
          "text-danger"
          , "Error"
          , `Failed to load: ${status}, ${error}`
        )
      }
      , complete: (xhr, status) => {
        submitBtnEl
          .prop("disabled", false)
          .empty()
          .append(submitBtnText)
        // Handle complete (success or error)
      }
    })
  }
</script>
<!-- end form submit javascript -->
');
```

# DataTables

``` html
htp.p('
<table id="_table" class="table"></table>
');

htp.p('
<!-- start DataTables javascript -->
<script type="text/javascript">
  const TableEl = $("#_table") // add name of table
  let Table // add name of table

  const initTable = () => {
    Table = TableEl.DataTable({ // add name of table
      "ajax": {
        "type": "GET" // change if needed
        , "url": `` // ajax function
        , "dataSrc": (response) => {
          if (response.status == "success") {
            return response.data // or data function
          }
          showToast(
            "text-danger"
            , "Error"
            , `Failed to load. ${(response.sqlerrm) ? response.sqlerrm : ""}`
          )
        }
      }
      , pageLength: 50
      , order: [[0, "desc"]] // change if needed
      , dom: "BQftrip" // buttons, searchBuilder, search, table, processing, information, pagination
      , columnDefs: [
        { targets: "_all", defaultContent: "-"}
      ]
      , columns: [
        { "data": "", title: "" } // add columns as needed
      ]
    })
  }

  $(() => initTable())
</script>
<!-- start DataTables javascript -->
');
```
