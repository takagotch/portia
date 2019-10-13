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

```

```
```

```
```


