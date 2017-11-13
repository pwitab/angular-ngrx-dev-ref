.. _state:

Application State
=================

State Best Practices
--------------------

Structure the state after the frontend application, not the backend API.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The state should help you structure the data in the front end so it might not
be the best to model it after how the data is represented in the backend.

Make the state as flat as possible
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Having a nested state will introduce complexity. By making the state flat we
will have a more maintainable solution

Use maps instead of arrays
^^^^^^^^^^^^^^^^^^^^^^^^^^

We can treat our applications state as an in memory database. We want to query
the database and then it is not efficient to  go through arrays to find the
data we need.
But if it represented with a map we can use it easier.

.. code-block:: TypeScript
   :linenos:

   // with Map
    objects = {
       1: {'name': 'Norrlands Guld', 'type': 'lager'},
       2: {'name': 'Guinness', 'type': 'stout'},
       3: {'name': 'Tuborg', 'type': 'lager'}
   }

   // with Array
   objects = [
      {'id': 1, 'name': 'Norrlands Guld', 'type': 'lager'},
      {'id': 2, 'name': 'Guinness', 'type': 'stout'},
      {'id': 3, 'name': 'Tuborg', 'type': 'lager'}
   ]


Don't model the state for use in views
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It might be a quickfix to model the state of your application after how you
want to present it to the user. Different views might want to display the same
data in different ways. By modeling your data for the views you might end up
duplicating data in your state

Don't duplicate data in your state
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You want your state to be the single source of truth. When we have dupliated
data in the state will have to update several parts every time and eventually
this will increase complexity and introduce errors and bugs.
Make use of selector functions and custom filters to get the data you need.


Don't store derived data in your state
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Again with the single source of truth. If we store derived state we have to
keep it updated and this will increase complexity and might intrdduce bugs.


Normalize your data
^^^^^^^^^^^^^^^^^^^

We want to reduce the complexity and keep the state flat. It will be easier to
maintain and add new functionality.

.. code-block:: TypeScript
   :linenos:

   // denormalized
    users = {
     '1': {'name': 'Oscar',
          'registered_at': '2017-02-01T09:45:32',
          'groups': [
             {'id': 1, 'name': 'admin', 'description': 'Group for admins'}]
           },
     '2': {'name': 'Sara',
          'registered_at': '2017-05-04T10:20:42',
          'groups': [
             {'id': 1, 'name': 'admin', 'description': 'Group for admins'},
             {'id': 2, 'name': 'editor', 'description': 'Group for editors'}  ]
           },
     '3': {'name': 'Robert',
          'registered_at': '2017-12-01T14:45:40',
          'groups': [
             {'id': 2, 'name': 'editor', 'description': 'Group for editors'}]
           },
   }

   // normalized
   users = {
     '1': {'name': 'Oscar',
          'registered_at': '2017-02-01T09:45:32',
          'groups': ['1']
           },
     '2': {'name': 'Sara',
          'registered_at': '2017-05-04T10:20:42',
          'groups': ['1', '2']
           },
     '3': {'name': 'Robert',
          'registered_at': '2017-12-01T14:45:40',
          'groups': ['2']
           },
   }

   groups = {
      '1': {'name': 'admin', 'description': 'Group for admins'},
      '2': {'name': 'editor', 'description': 'Group for editors'},

   }

