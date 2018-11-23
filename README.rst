mans - mocks are not stubs
==========================


Installing
----------


.. code:: bash

   pip install mans


A python library that makes explicit distiction about different types
of test doubles.

Stubbing
--------


.. code:: python


   import mans


   class S3Storage:
       def __init__(self, bucket_name):
           self.bucket_name = bucket_name
           # I/O side-effect in the constructor
           self.s3 = boto3.client('s3')
           self.bucket = self.s3.bucket(bucket_name)


   class File:
       def __init__(self, filename):
           self.filename = filename

       def calculate_name(self, storage: S3Storage):
           if not isinstance(storage, S3Storage):
               msg = f'expects a S3Storage but got {storage} {type(storage)} instead'
               raise TypeError(msg)

           return f'{storage.bucket_name}/{self.filename}'

   storage = mans.stub(S3Storage, bucket_name='foobar')

   result = File('chucknorris.txt').calculate_name(storage)

   result.should.equal('foobar/chucknorris.txt')
