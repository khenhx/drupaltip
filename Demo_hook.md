## Bài tập: 1
## Cách một: Hook vào update_node_form để thêm một số tính năng
```php
function demo_hook_form_alter(&$form, &$form_state, $form_id) {
//Hook $form_id để chọn duy nhất mọt form.
vd:  if ($form_id === 'update_node_form') {
        $form['demo1'] = [
        '#type' => 'textfield',
        '#title' => t('Tên'),
        '#size' => 14,
        '#maxlength' => 4,
        '#weight' => -10,
        ];

//Hook vào submit để đưa hàm lên đầu, ta dùng lệnh '#weight'.
vd:    $form['submit']['#weight'] = -15;

//Hook vào content_types để thay đôi #title.
vd:    $form['content_types']['#title'] = t('Chọn content type');

//Hook vào update để thay đổi #title.
vd:    $form['update']['#title'] = t('Nhập  nội dung');
  }
}
```


## Cách 2: Thay vì gọi hàn if riêng như cách một thì C2 mình add vào function luôn.
```php
//Cách này mik hook thẳng vào function để gọi tới form.
vd:    function demo_hook_form_update_node_form_alter(&$form, &$form_state, $form_id) {
    $form['demo1'] = [
        '#type' => 'textfield',
        '#title' => t('test'),
        '#size' => 20,
        '#maxlength' => 40,
        '#weight' => -10,
    ];

//Hook vào submit để đưa hàm lên đầu, ta dùng lệnh '#weight'.
vd:    $form['submit']['#weight'] = -15;

//Hook vào content_types để thay đôi #title.
vd:    $form['content_types']['#title'] = t('Chọn content type');

//Hook vào update để thay đổi #title.
vd:    $form['update']['#title'] = t('Nhập  nội dung');
}
```


## Bài tập 2: Add style button: Tạo
## Chuyển đổi chứ HOA thành chữ thường, màu chữ. cỡ chữ.
## Chúng ta tạo một folder dật têm với đuôi là css
```php
form-submit {
  text-transform: uppercase;
  font-size: 150% !important;
  color: #2f1e7f !important;
}
```


## Bài tập 3 Thêm element text field: Custom search.
## Cấu trúc: Chuyển tất cả chữ hoa thành chữ thường, gắn giá trị vào elemant custom bằng cách dùng jquery để tương tác.
## Đầu tiên thêm một element text field với name: Custom search
```php
  $form['custom_search'] = [
    '#type' => 'textfield',
    '#title' => t('Custom search'),
    '#size' => 80,
    '#maxlength' => 255,
  ];
```

## Tạo  một folder với tên có đuôi: js rồi gắn giá trị dưới vào.
```javascript
vd:
(function($, Drupal) {
  'use strict';
  Drupal.behaviors.createNode = {
    attach: function(context) {
      function textthaydoi() {
        var value = $(this).val();
        var valueTemp = value.toLowerCase();
        $("#edit-custom-search").val(valueTemp);
        // $("#edit-aos").val(value);
      }
      $("#edit-name").keyup(textthaydoi);
    }
  };
})(jQuery, Drupal);
```


## Bài tập: 4 Thêm custom submission handler cho form để xử lý:
## Đầu tiên mink gọi hàm  rồi dặt tên cho nó:
## Thêm custom submission handler **https://drupal.stackexchange.com/questions/223342/add-a-custom-submission-handler-to-a-form**
## Tạo term **https://drupal.stackexchange.com/questions/108868/programmatically-create-a-term**
```php
$form['submit']['#submit'][] = '_demo_hook_form_submit';
```
## Sau đó mink gọi hàm function rồi gán các giá trị vào. 
```php
function _demo_hook_form_submit(&$form, FormStateInterface $form_state) {
  $customSearch = $form_state->getValue('custom_search');
  $categories_vocabulary = 'search_node'; // Vocabulary machine name
  $categories = explode(" ", $customSearch);; // List of test terms
  foreach ($categories as $category) {
    $term = Term::create([
      'parent' => [],
      'name' => $category,
      'vid' => $categories_vocabulary,
    ])->save();
  }
}
```


## Bài tập: 5 Thêm validate cho Element nhập số lượng bài viết cần tạo.
## Tạo một validate với tên:
```php
$form['#validate'][] = '_Tên_mà_mày_thik_đặt';
```
## tạo function gọi tên của validae xuống xong gét các biến vào ta đk một hàm  <chứ đầu tiên phải viết hoa>.
```php
function _demo_hook_validate(&$form, FormStateInterface $form_state) {
  $name = $form_state->getValue('name');
  if ($name != ucfirst($name)) {
    $form_state->setErrorByName('name', t('Chữ đầu tiên phải viết hoa!'));
  }
```
## Tương tự mink làm cho hàm <hệ điều hành>.
```php
  $aos = $form_state->getValue('aos');
  if ($aos != ucfirst($aos)) {
    $form_state->setErrorByName('aos', t('Chữ đầu tiên phải viết hoa!'));
  }
```
## Hàm này mink sét ko đk quá tối đa 2 từ, mình dùng (count(explode(" ", $Tên biến)) để tách từ ra bằng khoảng cách, rồi gét các giá trị vào ta đk hàm <ko quá tối đa 2 từ>.
```php
  if (count(explode(" ", $aos)) > 2) {
    $form_state->setErrorByName('aos', t('Chỉ đc chứa tối da 2 từ'));
  }
}
```
