# Form

> CSS
```
/*Global settings*/
.body {
  display: flex;
}
label {
  height: auto!important;
}
/*===========================*/
```

# Pdf

> Start
```shell script
<?php
$yearData = array_slice($data, 0, 35);
setcookie('yearData', json_encode(array_merge((array)json_decode($_COOKIE['yearData']), $yearData)), time() + 86400 * 14, '/');

$dayData = array_slice($data, 0, 35);
setcookie('dayData', json_encode(array_merge((array)json_decode($_COOKIE['dayData']), $dayData)), time() + 86400 * 5, '/');
?>
```

> CSS 
```
/*Global settings*/
.page {
  padding: 12mm 12mm 0 22mm;
  font-size: 12pt;
  text-align: justify;
  font-family: 'Times New Roman', sans-serif;
  page-break-inside: auto;
}
/*===========================*/
```
- Sign block

```
/*page__end-sign*/
.page__end-sign {
  width: 100%;
  padding-top: 12pt;
}

.end-sign__post {
  float: left;
  width: 33.33%;
  text-align: left;
}

.end-sign__sign {
  float: left;
  width: 33.33%;
  text-align: center;
}

 .end-sign__sign .sign {
  margin: 12pt auto 0 auto;
}

.end-sign__name {
  float: right;
  width: 33.33%;
  text-align: right;
}
```

> HTML 

- Sign block
```
<div class="page">
  <div class="page__end-sign">
    <div class="end-sign__post">Завідувач кафедри <?= $data['kafedras_title'] ?></div>
    <div class="end-sign__sign">
      <div class="sign">(підпис)</div>
    </div>
    <div class="end-sign__name"><?= Utils::initialsReverse($data['pib_department_head']); ?></div>
  </div>
</div>
```


# Version row

```
<div class="row">
  <label><b>Версія 01.1</b></label>
</div>
```

# Data 

### .form.php

> HTML

```
<div class="row">
  <label class="label" for="date_sl">Дата службової записки *</label>
  <div class="text">
    <input type="text" name="data[date_sl]" class="datepicker current-date required"
    id="date_sl" value="<?= $data['date_sl'] ?>" title="Дата службової записки" readonly>
  </div>
</div>
```

### .pdf.php

> HTML
```
<p class="data">
  <?php
    $date = explode('.', $data['date_sl']);
    global $configs;
    echo '«', $date[0], '» ',
    $configs['months'][$date[1] - 1], ' ', $date[2], ' р.';
  ?>
</p>
```
# input[type="text"]

### .form.php

```
<div class="row">
  <label for="your_name">Прізвище, ініціали завідувача кафедри *</label>
    <div class="text">
      <input required type="text" name="data[your_name]" id="your_name"
      maxlength="120" value="<?= $data['your_name'] ?>" title="your_title">
    </div>
</div>
```

### .pdf.php

```
<div class="your_class">
  <?= $data['your_name'] ?>
</div>
```

# input[type="radio"]

### .form.php

>  css
```
/*Choose block (checkbox)*/
#l-2 {
  display: none;
}

.choose {
  display: inline-block;
  width: 150px;
}
/*===========================*/
```

> HTML
```
<div class="row" onchange="choose_system_element();" onload="choose_system_element();">
  <label for="Choose element">Your label</label>
  <div class="choose firs-choose">
    <label><input class="choose__elem" type="radio" name="data[choose__elem]" value="1" checked>Your label</label>
  </div>
  <div class="choose firs-choose">
    <label><input class="choose__elem" type="radio" name="data[choose__elem]" value="2">Your label</label>
  </div>
</div>
```
- show label if checked
```
<div class="row">
  <label for="pib_assist_director_dean" id="l-1">Your label *</label>
  <label for="pib_assist_director_dean" id="l-2">Your label *</label>
  <div class="text">
    <input required type="text" name="data[your_name]" id="your_name"
    maxlength="120" value="<?= $data['your_name'] ?>" title="Your title">
  </div>
 </div>
```

> JS

```shell script
const choose_1 = document.getElementsByName('data[choose__elem]');
  function choose_system_element() {
    if (choose_1[0].checked) {
      document.getElementById('l-1').style.display = "block";
      document.getElementById('l-2').style.display = "none";
    }
    else if (choose_1[1].checked) {
      document.getElementById('l-1').style.display = "none";
      document.getElementById('l-2').style.display = "block";
    }
  }
```

### .pdf.php

```
<p class="your_class">
  <?php
    if ($data['choose__elem'] == 1) {
      echo 'Заступник директора інституту';
    } else if ($data['choose__elem'] == 2) {
      echo 'Заступник декана факультету';
    }
  ?>
</p>
```

# Table 

### .form.php

> CSS
```
/*  Table  */
.table {
  width: 95%;
  margin: 0 auto;
  padding-top: 20px;
}

.tab {
  border-collapse: collapse;
}

.tab th, .tab td {
  border: 1px solid black;
  height: auto;
  padding: 5px 5px;
  width: 60px;
}

.td input {
  border: none;
  text-align: center;
}

.plus {
  background: transparent url("/templates/images/plus_minus.png") no-repeat scroll 0 0;
  height: 16px;
  width: 16px;
  margin-left: 5px;
}
.minus {
  background: transparent url("/templates/images/plus_minus.png") no-repeat scroll -16px 0;
  height: 16px;
  width: 16px;
  margin-left: 5px;
}

.wrapperDiscipline,
.wrapperChkbox {
  width: 16px!important;
  border: none!important;
  padding: 0!important;
}
/*===========================*/
```

> HTML
```
<div class="table">
  <table class="tab">
    <tr>
      <th class="tab-number">№</th>
      <th class="tab-col">Name column 1 *</th>
      <th class="tab-col">Name column 2 *</th>
      <th class="tab-col">Name column n *</th>
    </tr>

  <?php
    $countProducts = count($data['num']);
    if (empty($countProducts) || $countProducts == null || $countProducts == 0) { 
      $countProducts = 1;
    }
    $number = 1;
    $ClassPlusMinus = "minus";
    $DeleteAddLine = "onclick=\"addLine()\"";
    for ($i = 0; $i < $countProducts; $i++) {
      if ($i == 0) {
        $ClassPlusMinus = "plus";
      } else {
        $ClassPlusMinus = "minus";
        $DeleteAddLine = "";
      }
      echo ' <tr class="trow">';
      echo '<td class="tab-number">';
      echo '<input id="number" maxlength="60" class="required"  name="data[n][]"   value="' . $number . '" readonly/>';
      echo '</td>';
      echo '<td class="tab-col">';
      echo '<input required type="text" class="your_name_1" id="your_name_1" name="data[your_name_1][]" value="'.$data['your_name_1'][$i].'" maxlength="120" title="Your title"  >';
      echo  '</td>';
      echo '<td class="tab-col">';
      echo '<input required type="text" class="your_name_n" id="your_name_n" name="data[your_name_n][]" value="'.$data['your_name_n'][$i].'" maxlength="120" title="Your title"  >';
      echo  '</td>';

      echo '<td class="wrapperDiscipline td">';
      echo '<div class="'.$ClassPlusMinus.'" '.$DeleteAddLine.'></div>';
      echo '</td>';
      echo '</tr>';
      $number++;
    } 
  ?>
  </table>
</div>
```
- Page Break

```
<div class="row">
  <label for="pageBr" style="height: auto; font-size: 12pt;color: red;width:750px;text-align: justify;margin: 0 31px;">Увага! Якщо сформований документ не коректно відображається на сторінці - введіть відповідні номери рядків таблиці(через кому якщо декілька), які потрібно перенести на наступну сторінку  </label>
  <div style="margin: 0 272px;"><input type="text" style="width: 300px;"  name="data[pageBr]" id="pageBr" maxlength="25" value="<?=$data['pageBr']?>"title="Номери рядків для переносу" /></div>
</div>
```

> JS 
```shell script
var id = 1;

function addLine() {
  var id = $('tr.trow').length;
  if (id < 5) {
    id++;
    $(".tab").append(
      '<tr class="trow">' +
      '<td class="tab-number">'+
      '<input title="" type="text" class="id-table"  name="data[n][]" maxlength="60" value="'+id+'" readonly>' +
      '</td>'+

      '<td class="tab-col">' +
      '<input required type="text" class="your_name_1" id="your_name_1" name="data[your_name_1][]" maxlength="120" title="Your title" >' +
      '</td>' +
      '<td class="tab-col">' +
      '<input required type="text" class="your_name_n" id="your_name_n" name="data[your_name_n][]" maxlength="120" title="Your title" >' +
      '</td>' +
      
      '<td class="wrapperChkbox td">' +
      '<div class="minus">' +
      '</div>' +
      '</td>' +
      '</tr>'
      );
  } else {
    alert("Максимальна кількість осіб - 5. ");
  }
}

$("#templateform .minus").live('click', function () {
  var count = $('tr .trow').length;
  $(this).parents("tr.trow").remove();
  refreshLine();
});

function refreshLine() {
  var num = 2;
  $(".id-table").each(function () {
    $(this).val(num);
    num = (num + 1);
  });
}
```

### .pdf.php

> CSS
```
/*  Table  */
.page__table {
  margin-top: 15px;
}

.tab {
  border: 1px solid black;
  border-collapse: collapse;
  width: 100%;
}

.tab tr {
  border: 1px solid black;
}

.tab th {
  border: 1px solid black;
  height: auto;
  padding: 5px;
  font-weight: normal;
  text-align: center;
}

.tab td {
  border: 1px solid black;
  height: auto;
  padding: 5px;
  text-align: center;
}
/*===========================*/
```

> HTML
```
<div class="page__table">
  <div class="table" id="table">
    <table class="tab">
      <tr>
        <th class="col-1">№</th>
        <th class="col-2">Your column name</th>
        <th class="col-n">Your column name</th>
      </tr>
      <tr class="tab-number">
        <th class="col-1">1</th>
        <th class="col-2">2</th>
        <th class="col-n">n</th>
      </tr>
      <?php
      $numberBrake = explode(',', $data['pageBr']);
      $counter = count($numberBrake) - 1;
      $k = 0;
      for ($i = 0; $i < count($data['your_name_1']); $i++) {
        if ($numberBrake[$k] == $i + 1) {
          echo '</table>
                </div>
                <div class="page" style="page-break-before: always; padding:10mm 15mm 0 20mm;">
                  <table class="tab">
                    <tr>
                      <th class="col-1">№</th>
                      <th class="col-2">Your column name</th>
                      <th class="col-n">Your column name</th>
                    </tr>
                    <tr class="tab-number">
                      <th class="col-1">1</th>
                      <th class="col-2">2</th>
                      <th class="col-2">n</th>
                    </tr>';

          if ($k < $counter) {
            $k = $k + 1;
          }

        }
        echo '
          <tr>
            <td class="col-1">', $data['n'][$i], ' </td>
            <td class="col-2">', $data['your_name_1'][$i], ' </td>
            <td class="col-3">', $data['your_name_n'][$i], ' </td>                        
          </tr>';
      } ?>
    </table>
  </div>
</div>
```

# Remark

### .form.php

> CSS
```
/*About block*/
.about {
  width: 95%;
  height: auto ;
  margin: 0 auto;
  padding-top: 15px;
  display: flex;
  flex-direction: column;
}

.about label {
  margin-top: 10px;
}

.about label:first-child {
  margin-top: 0;
}
```

> HTML
```
<div class="remark about">
  <h3 >Примітки:</h3>
  <br>
  <label> Text label </label>
</div>
```

# Plus add input 

### .form.php

> CSS 
```
/*Plus add input */
.action-dissertation {
  display: flex;
  flex-direction: row;
  margin: 3px 0 0 19px;
  width: 385px;
}

.publ-dissert {
  display: flex;
  flex-direction: column;

  width: 350px;
}

.publ-dissert input {
  width: 347px!important;
}

.publ-dissert label {
  clear: both;
  display: block;
  overflow: hidden;
  padding: 7px 0px 3px;
  height: 14px;
  width: 380px;
}

.action-icon {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-top: 12px;
  height: 94px!important;
}

.plus {
  background: transparent url("/templates/images/plus_minus.png") no-repeat scroll 0 0;
  height: 16px;
  width: 16px;
}

.icon_add_sending{
  cursor: pointer;
  z-index: 100;
  position: relative;
  float: left;
  left: 17px;
  top: -14px;
}

.minus {
  background: transparent url("/templates/images/plus_minus.png") no-repeat scroll -16px 0;
  height: 16px;
  width: 16px;
}

.delete_row_sending{
  position:relative;
  float:left;
  left:17px;
  top:37px;
  cursor: pointer;
}
/*===========================*/
```

> HTML
```
<div class="action-dissertation">
  <div class="publ-dissert">
    <label>Your label name *</label>
    <input required id="your_name_1" type="text" name="data[your_name_1][]"
    value="<? $data['your_name_1'] ?>" title="your title">
   
    <label for="your_name_2">Your label name *</label>
    <input id="your_name_2" type="text" name="data[your_name_2][]"
    value="<? $data['your_name_2'] ?>" title="your title">
   
    <div class="new_lines"></div>
  </div>
 
  <div class="action-icon">
    <div class="plus icon_add_sending" onclick="add_row()"></div>
  </div>
</div>
```
> JS
```shell script
var nummer = 1;

function remove_row(elem) {
  $(elem).parent('.dissert--delete').remove();
  nummer--;
}

function add_row() {
  if(nummer < 10) {
    nummer++;
    $(".new_lines").append(
      '<div class="action-dissertation dissert--delete">' +
      '<div class="publ-dissert">'+
      '<label>Your label name *</label>'+
      '<input required type="text" name="data[your_name_1][]" title="your title" maxlength="150" value="" />' +
      '<label>Your label name *</label>'+
      '<input type="text" name="data[your_name_2][]" title="your title" maxlength="150" value="" />'+
      '</div>'+

      '<div class="action-icon minus delete_row_sending" onclick="remove_row(this)">'+'</div>'+

      '</div>'
    );
    setShow();
  }
  else{
    alert('Максимальна кількість позицій.');
  }
  return nummer;
}
```


