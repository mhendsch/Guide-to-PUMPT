####################################
Making User Models
####################################

This section will discuss how to create a learner model using the Personis System.

First, we have to understand what makes up the Personis system. The Personis system can be visualized as a tree.
Each leaf, called a *component*, contains information about the user. 

Components are made up of a list of *evidence* values. When the source application (the source of the evidence) makes new evidence available, 
it *tells* the user model and the evidence is added to the component. 

When an application needs to know information from the model, it *asks* the model, which then returns a value. 


Creating a model
---------------------

To create a user model, first cd into **/B2A/PUML** and run:

.. code-block:: python
    
    python3 -m venv Personis


Then run: 

.. code-block:: python

    source Personis/bin/activate

This will create a virtual environment to allow you to access the necessary python modules. Once you've 
done this, cd into **/B2A/PUML/Personis/Personis** and run:

.. code-block:: python

    ./umbrowser.py

The result should look similar to:

:: 

    $./umbrowser.py
    [''] >

This prompt shows that the umbrowser is waiting for a command. 


.. warning::
    
    If you get a "no module named Personis" or something similar when running umbrowser.py, then before you run the ``source Personis/bin/activate`` run:

    ``export PYTHONPATH=$PYTHONPATH:/home/ubuntuUsername/B2A/PUML/Personis/Personis/Src``

To see the list of available commands, you can type:

.. code-block:: python

    [''] > help

In order to create a model, you first have to specify a username and password:

.. code-block:: python
   
    [''] > help login
    login username password
    [''] > login matt secret
    username: matt
    password: secret

Models can be stored in the local system or in a server. To specify which, you can specify either "base"
to store them locally or "server" to store them on a server. For the time bering, we'll store it on the
local machine by typing:

.. code-block:: python

    [''] > base

In order to make a model for Matt in the current directory (.), we can use the mkmodel command:

.. code-block:: python

    [''] > help mkmodel
    mkmodel model_name [model_directory]
    makes a new empty model
    uses username, password specified in login cmd
    [''] > mkmodel Matt .
    making model 'Matt' in directory '.' with username 'matt' and password 'secret'
    model made
    to access the model, use the command 'model Matt .'

To access the new model, we use the ``model`` command:

.. code-block:: python
    
    [''] > model Matt .
    2024-05-31 14:40:29,323 >>>>Access: Matt::.
    model 'Matt' open, access type is 'base'

Just like in Linux, we can use ls to list the items in the current directory:    
    
.. code-block:: python
    
    [''] > ls
    Components:
    N/A
    Contexts:
    []
    Views:
    {}
    Subscriptions:
    {}

The model is currently empty. We can add a context using the ``mkcontext`` command:

::

     [''] > mkcontext Prefs
    Context description? My preferences
    Create new context 'Prefs' in context '['']' with description 'My preferences'
    Ok?[N] Y
    [''] > ls
    Components:
    N/A
    Contexts:
    ['Prefs']
    Views:
    {}
    Subscriptions:
    {}

We can now cd into the **Prefs** context that we just made and make a new component for it, we'll call it "Food":

::

     [''] > cd Prefs

    Matt ['', 'Prefs'] > mkcomponent food
    Component description? type of food I prefer
    Component type:
    0 attribute
    1 activity
    2 knowledge
    3 belief
    4 preference
    5 goal
    Index? 4
    Value type:
    0 string
    1 number
    2 boolean
    3 enum
    4 JSON
    Index? 0
    Creating new component 'food', type 'preference', description 'type of food I prefer', value type 'string'
    Ok?[N] Y

    Matt ['', 'Prefs'] > ls
    ===================================================================
    Component:  type of food I prefer
    ===================================================================
    showobj:
    Identifier = food
    Description = type of food I prefer
    Explanation =
    component_type = preference
    value_type = string
    value_list = []
    value = None
    resolver = None
    goals = []
    evidencelist = 0 items
    objectType = Component
    creation_time = ........
    ---------------------------------
    Evidence about it
    ---------------------------------
    Components:
            food: type of food I prefer
    Contexts:
    []
    Views:
    {}
    Subscriptions:
    {}

    Matt ['', 'Prefs'] >

We can now add evidence to the food component using the ``tell`` command:

::

    Matt ['', 'Prefs'] > tell food
    Value? Italian
    Evidence type:
    0 explicit
    1 implicit
    2 exmachina
    3 inferred
    4 stereotype
    Index? [0]
    Evidence flag? (CR for none)
    Tell value=Italian, type=explicit, flags=[], source=mhendsch, context=['', 'Prefs'], component=food
    Ok?[N] Y
    Matt ['', 'Prefs'] > ls food
    ===================================================================
    Component:  type of food I prefer
    ===================================================================
    showobj:
    Identifier = food
    Description = type of food I prefer
    Explanation =
    component_type = preference
    value_type = string
    value_list = []
    value = Italian
    resolver = None
    goals = []
    evidencelist = 1 items
    objectType = Component
    creation_time = .........
    ---------------------------------
    Evidence about it
    ---------------------------------
    showobj:
        flags = []
        evidence_type = explicit
        source = mhendsch
        owner = mhendsch
        value = Italian
        comment = None
        creation_time = ...........
        time = None
        useby = None
        objectType = Evidence
    ---------------------------------


Congrats! We've now created a user model. You can type ``quit`` to quit umbrowser.

.. toctree::



