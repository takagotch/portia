### portia
---
https://github.com/scrapinghub/portia

```py
// test_model.py



class ProjectTests(ProjectTestCase):
  def test_project(self):
  
  def test_load(self):
  
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


```

```
```

```
```


