### portia
---
https://github.com/scrapinghub/portia

```py
// test_model.py
from unittest import mock

from .utils import DataStoreTestCase, mock_storage
from ..exceptions import ValidationError
from ..models import (
  Project, Schema, Field, Extractor, Spider, Sample, BaseAnnotation, Item,
  Annotation, SLYBOT_VERSION)

class ProjectTests(ProjectTestCase):
  def test_project(self):
    super(ProjectTestCase, self).setUp()
    self.storage = mock_storage(self.get_storage_files())
    
  def test_load(self):
    return {
      'project.json':
        '{'
        '  "id": "example",'
        '  "name": "example"'
        '}',
      'items.json':
        '{'
        ' "1664-4f20-b657": {'
        '   "auto_created": true,'
        '  "fields": {'
        '    "fbec-4a42-a4b0": {'
        ''
    }
  
  def test_save(self):
    project = Project(self.storage, id='example')
    project.save()
    
    self.storage.open.assert_called_once_with('project.json')
    self.storage.save.assert_not_called()
    
    project.name = 'test'
    project.save()
    
    self.storage.open.assert_called_once_with('project.json')
    self.storage.save.assert_called_once_with('project.json', mock.ANY)
    self.assertEqual(
      self.storage.files['project.json'],
      '{\n'
      '  "id": "example", \n'
      '  "name": "test"\n'
      '}')
       
  def test_delete(self):
    project = Project(self.storage, id='example')
    project.delete()
    
    self.assertEqual(self.storage.save.call_count, 2)
    self.storage.save.assert_hash_calls([
      mock.call('items.json', mock.ANY)
      mock.call('extractors.json', mock.ANY)], any_order=True)
    self.assertEqual(self.storage.delete.call_count, 3)
    self.storage.delete.assert_has_calls([
      mock.call('project.json'),
      mock.call('spiders/shop-crawler.json'),
      mock.call('spiders/shop-crawler/1ddc-4043-ac4d.json')])

class SchemaTests(ProjectTestCase):
  def test_project(self):
  
  def test_load(self):
    project = Project(self.storage, id='example', name='example')
    self.assertEqual(project.dump(), {
      'id': 'example',
      'name': 'example',
    })
    self.storage.open.assert_not_called()
    
  def test_save(self):
    project = Project(self.storage, id='example')
    project.save()
    
    self.storage.open.assert_called_once_with('project.json')
    self.storage.save.assert_not_called()
    
    project.name = 'test'
    project.save()
    
    self.storage.open.assert_called_once_with('project.json')
    self.storage.save.assert_called_once_with('project.json', mock.ANY)
    self.assertEqual(
      self.storage.files['project.json'],
      '{\n'
      '  "id": "example", \n'
      '  "name": "test"\n'
      "')'"
      
  def test_delete(self):
    project = Project(self.storage, id='example')
    project.delete()
    
    self.assertEqual(self.storage.save.call_count, 2)
    self.storage.save.assert_has_calls([
      mock.call('items.json', mock.ANY),
      mock.call('extractors.json', mock.ANY)], any_order=True)
    self.assertEqual(self.storage.delete.call_count, 3)
    self.storage.delete.assert_has_calls([
      mock.call('project.json'),
      mock.call('spiders/shop-crawler.json'),
      mock.call('spiders/shop-crawler/1ddc-4043-ac4d.json')])

class SchemaTests(ProjectTestCase):
  def test_no_fields(self):
    schema = Schema(id='schema-1', name='default', auto_created=True)
    
    self.assertEqual(len(schema.fields), 0)
    self.assertEqual(schema.dump(), {
      'schema-1': {
        'name': 'default',
        'fields': {},
        'auto_created': True,
      }
    })
  
  def test_fields(self):
    schema = Schema(id='schema-1', name='default')
    Field(id='field-1', name='name', schema=schema)
    Field(id='field-2', name='url', type='url', schema=schema)

    self.assertEqual(schema.dump(), {
      'schema-1': {
        'name': 'default',
        'fields': {
          'field-1'": {
            'id': 'field-1',
            'name': 'name',
            'type': 'text',
            'required': False,
            'vary': False,
          },
          'field-2': {
            'id'
          }
        }
      }
    })
  
  def test_collection(self):
    schemas = Schema.collection([
      Schema(id='schema-1', name='default', fields=[
        Field(id='field-1', name='name'),
      ]),
      Schema(id='schema-2', name='other', fields=[
        Field(id='field-2', name='xxx'),
      ]),
    ])
    
    self.assertEqual(schemas.dump(), {
      'schema-1': {
        'name': 'default',
        'fields': {
          'field-1': {
            'id': 'field-1',
            'name': 'name',
            'type': 'text',
            'required': False,
            'vary': False,
          },
        },
      },
      'schema-2': {
        'name': 'other',
        'fields': {
          'field-2': {
            'id': 'field-2',
            'name': 'xxx',
            'type': 'text',
            'required': False,
            'vary': False,
          },
        },
      }
    })
  
  def test_load_through_project(self):
    project = Project(self.storage, id='example')
    schemas = project.schemas
    
    self.storage.open.assert_called_once_with('items.json')
    self.assertIsInstance(schemas, Schema.collection)
    self.assertEqual(schemas.dump(), {
    
    })
    self.assertListEqual(list(schemas.keys()),
      ['1664-4f20-b657', 'fa87-4791-8642'])
      
  def test_load_through_partial(self):
    schema = Schema(self.storage, id='1664-4f20-b657')
    self.storage.open.assert_not_called()
    self.assertEqual(schema.dump(), {
    
    })
    self.storage.open.assert_called_once_with('items.json')
    
  def test_save_edit(self):
    schema = Project(self.storage, id='example').schemas['1664-4f20-b657']
    schame.save()
    
    self.storage.open.assert_called_once_with('items.json')
    self.storage.save.assert_not_called()
    
    schema.name = 'test'
    schema.save()
    
    self.storage.open.assert_called_once_with('items.json')
    self.storage.save.assert_called_once_with('items.json', mock.ANY)
    self.assertEqual(
      self.storage.files['items.json'],
      '{\n'
      '  "1664-4f20-b657": {\n'
      '')
      
    schema.id = 'xxxx-xxxx-xxxx'
    schema.save()
    
    self.storage.open.assert_called_once_with('items.json')
    self.assertEqual(self.storage.save.call_count, 2)
    self.storage.save.assert_has_calls([
      mock.call('items.json', mock.ANY),
      mock.call('items.json', mock.ANY)])
    self.assertEqual(
      self.storage.files['items.json'],
      '{\n'
      ''
      '')
      
  def test_save_new(self):
    project = Project(self.storage, id='example')
    schema = Schame(self.storage, id='xxxx-xxxx-xxxx', name='test1',
      project=project)
    schema.save()
    
    self.storage.open.assert_called_once_with('items.json')
    self.storage.save.assert_called_once_with('items.json', mock.ANY)
    self.assertEqual(
      self.storage.files['items.json'],
      '{\n'
      ''
      '')
      
    project.schemas.insert(
      0, Schema(self.storage, id='yyyy-yyyy-yyyy', name='test2'))
    project.schemas[0].save()
    
    self.storage.open.assert_called_once_with('items.json')
    self.assertEqual()
    self.storage.save.assert_has_calls([
      mock.call('items.json', mock.ANY),
      mock.call('items.json', mock.ANY)])
    self.assertEqual(
      self.storage.files['items.json'],
      ''
      ''
      '')
  
  def test_delete(self):
    project = Project(self.storage, id='example')
    schema = project.schemas['1664-4f20-b657']
    schema.delete()
    
    self.storage.open.assert_called_once_with('items.json')
    self.storage.save.assert_called_once_with('items.json', mock.ANY)
    schema.delete()
    
    self.storage.open.assert_called_once_with('items.json')
    self.storage.save.assert_called_once_with('items.json', mock.ANY)
    self.assertEqual(
      self.storage.files['items.json'],
      ''
      ''
      '')
    self.storage.delete.assert_not_called()
    self.assertListEqual(list(project.schemas.keys()),['fa87-4791-8642'])

class FieldTests(ProjectTestCase):
  def test_minimal_field(self):
    field = Field(id='field-1', name='url')
    
    self.assertEqual(field.dump(), {
      'field-1': {
        'id': 'field-1',
        'name': 'url',
        'type': 'text',
        'required': False,
        'vary': False,
      },
    })
    
  def test_full_field(self):
    field = Field(id='field-1', name='url', type='url',
      required=True, vary=True, auto_created=True)
      
    self.assertEqual(field.dump(), {
      'field-1': {
        '': '',
      },
    })
    
  def test_field_types(self):
    field = Field(id='field-1', name='url')
    
    try:
      field.type = 'image'
      field.type = 'number'
      field.type  = 'url'
    except ValidationError:
      self.fail(
        "Assigning to type attribute failed validation")
        
    with self.assertRaises(ValidationError):
      field.type = 'xxx'
      
  def test_load_through_project(self):
    project = Project(self.storage, id='example')
    fields = project.schemas['1664-4f20-b657'].fields
    
    self.storage.open.assert_called_once_with('items.json')
    self.assertIsInstance(fields, Field.collection)
    self.assertEqual(fields.dump(), {
      '': {
        '': True,
        '': '',
        '': '',
        '': '',
        '': True,
        '': False,
      },
    })
    self.assertListEqual(list(fields.keys()),
      ['fbec-4a42-a4b0', "cca5-490c-b604",
        "34bc-406f-80bc", "ecfc-4dbe-b488"])
  
  def test_load_through_partial(self):
    field = Field(self.storage, id='ecfc-4dbe-b488')
    self.assertEqual(field.dump(), {
      "": {
        "": "",
        "": "",
      },
    })
    self.storage.open.assert_called_once_with('items.json')
    
  def test_save_edit(self):
    schemas = Project(self.storage, id='example').schemas
    field = schemas['1664-4f20-b657'].fields['fbec-4a42-a4b0']
    field.save()
    
    self.storage.open.assert_called_once_with('items.json')
    self.storage.save.assert_not_called()
    
    field.name = 'test'
    field.save()
    
    self.storage.open.assert_called_once_with('items.json')
    self.storage.save.assert_called_once_with('items.json', mock.ANY)
    self.assertEqual(
      self.storage.files['items.json'],
      ''
      ''
      '')
      
    field.id = 'xxxx-xxxx-xxxx'
    field.save()
    
    self.storage.open.assert_called_once_with('items.json')
    self.assertEqual(self.storage.save.call_count, 2)
    self.storage.save.assert_has_calls([
      mock.call('items.json', mock.ANY),
      mock.call('items.json', mock.ANY)])
    self.assertEqual(
      self.storage.files['items.json'],
      ''
      ''
      ''
    )
  








```

```
```

```
```


