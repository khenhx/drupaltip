## DemoSettingsForm: 
```yml
khenhx_form.settings:
  path: '/admin/khen-test'
  defaults:
    _form: '\Drupal\khenhx_form\Form\DemoSettingsForm'
    _title: 'Khen Test'
  requirements:
    _permission: 'access administration pages'
```


## EditSettingsForm:
```yml
khenhx_form.edit_info:
  path: '/admin/edit-info'
  defaults:
    _form: '\Drupal\khenhx_form\Form\EditSettingsForm'
    _title: 'Thong tin'
  requirements:
    _permission: 'access administration pages'
```


## GroupSettingsForm:
```yml
khenhx_form.group_info:
  path: '/admin/group-info'
  defaults:
    _form: '\Drupal\khenhx_form\Form\GroupSettingsForm'
    _title: 'group'
  requirements:
    _permission: 'access administration pages'
```


## CreateUserForm:
```yml
khenhx_form.create_user:
  path: '/create-user'
  defaults:
    _form: '\Drupal\khenhx_form\Form\CreateUserForm'
    _title: 'CreateUserForm'
  requirements:
    _permission: 'access content'
```


## CreateUserByRoleForm:
```yml
khenhx_form.create_user_by_role:
  path: '/create-user-by-role'
  defaults:
    _form: '\Drupal\khenhx_form\Form\CreateUserByRoleForm'
    _title: 'CreateUserForm'
  requirements:
    _permission: 'access content'
```


## DeleteUserByRoleForm:
```yml
khenhx_form.delete_user_by_role:
  path: '/delete-user-by-role'
  defaults:
    _form: '\Drupal\khenhx_form\Form\DeleteUserByRoleForm'
    _title: 'DeleteUserForm'
  requirements:
    _permission: 'access content'
```


## CreateNodeForm:
```yml
khenhx_form.create_node:
  path: '/create-node'
  defaults:
    _form: '\Drupal\khenhx_form\Form\CreateNodeForm'
    _title: 'CreateNodeForm'
  requirements:
    _permission: 'access content'
```


## ContentTypeForm:
```yml
khenhx_form.content_type:
  path: '/content-type'
  defaults:
    _form: '\Drupal\khenhx_form\Form\ContentTypeForm'
    _title: 'contentTypeForm'
  requirements:
    _permission: 'access content'
```


## DeleteNodeForm:
```yml
khenhx_form.delete_node:
  path: '/delete-node'
  defaults:
    _form: '\Drupal\khenhx_form\Form\DeleteNodeForm'
    _title: 'DeleteNodeForm'
  requirements:
    _permission: 'access content'
```


## DeleteMuitiNodeForm:
```yml
khenhx_form.delete_multi_node:
  path: '/delete-multi-node'
  defaults:
    _form: '\Drupal\khenhx_form\Form\DeleteMuitiNodeForm:'
    _title: 'DeleteNodeForm'
  requirements:
    _permission: 'access content'
 ```
 
 
## UpdateNodeForm:
```yml
khenhx_form.update_node:
  path: '/update-node'
  defaults:
    _form: '\Drupal\khenhx_form\Form\UpdateNodeForm'
    _title: 'UpdateNodeForm'
  requirements:
    _permission: 'access content'
```
