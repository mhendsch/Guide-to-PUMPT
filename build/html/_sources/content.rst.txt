############################################
Making Content
############################################

In order to make content, we use Restructured Text Language. A comprehensive guide to the most common
functionalities of Sphinx can be found on the `Sphinx Website <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`_.
However, I will cover the basics of creating an e-textbook.

Creating the Project
========================

First, in Ubuntu, run:

.. code-block:: python 

    sphinx-quickstart docs

When it asks if you want to separate source and build directories, type "y" (without quotes) then press enter.

Next, when it asks for the Project Name, input what you want to name your textbook. 

Write your name for the Author Name.

For Project Release, it can be any number, but you should probably start with "0.1".

When it prompts you for the language, press enter to stay with the default (English).

After these, prompts, a docs folder will be created, with build and source folders, as well as the makefiles for the textbook. You'll notice that this is the same as it is in the example textbook. 

To render the textbook as an HTML for the first time, type:

.. code-block:: python 

    sphinx-build -M html docs/source/ docs/build/


In your Ubuntu folder, you can go to **/docs/build/html** and double click the index.html file in order to open it in a browser. 


Writing Text
==================

Navigate to **docs/source** and open index.rst in a text or code editor. 

Modify it to look as follows:

.. code-block:: RST

    Welcome to Guide to Creating an E-textbook using Pumpt's documentation!
    =======================================================================


    You can write here and it will appear in your HTML file! The ====== denotes
    that the text above is a heading. There have to be at least as many "=" as 
    there are characters in the heading. If I wanted to emphasize that I would 
    surround it with *asterisks (This will usually make it italic).* Similarly
    if I wanted to strongly emphasize it, I would use **double asterisks (This 
    will typically make it bold).**
    Pressing enter will not start a new paragraph unless you press it twice.

    This will be a new paragraph.

    Subsection 1
    ----------------

    The ----- indicates that the text above is a subsection heading. 


Once you've done this, save it and then cd into docs. Then, type:

.. code-block:: python 

    make html

Once you do this, index.html will be updated. Refresh your browser or reopen the file to see your changes.
You'll notice that underlining text with "====" or "-----" will make it a heading, while encasing it with asterisks
will emphasize it. 

Adding Images
================

Now that you know how to write in your textbook, let's cover how to add images. 

Fortunately, RST makes it pretty easy to add images:

.. code-block:: RST

    .. image:: ImagePath/myImage.png
        :width: 10cm
        :align: center


The ``.. image::`` is called a *directive* and will display the image at ImagePath. I recommend creating a new folder in **docs/source** called "Images".
You can then put images into this folder, and reference it with the image directive. For example, if you had an image called "pictureDay2022.png" in the 
Images folder, your directive would look like this:


.. code-block:: RST

    .. image:: Images/pictureDay2022.png
        :width: 10cm
        :align: center


There are several parameters you can specify in the image directive. In the above example, I specify the alignment of the image as well as its width. 
You can also specify the height of the image using the ``:height:`` parameter. You should be careful to only specify width **or** height, as specifying both
can stretch the image. 

Once you add the image direcitve to your textbook, once again update the HTML by typing:

.. code-block:: python

    make html


Adding Pages
===============

Most textbooks have more than one page, so in this section we'll cover how to add pages to your textbook.

In **docs/source**, create another rst file. You can name it whatever you want, but for simplicity I'll call it **myContent.rst**.

Modify the file however you want:

.. code-block:: RST

    My Content Here
    ==================
    
    This is my second page! How exciting.

    This is a subheading
    ------------------------

    Here's some more content.

    .. image:: Images/myPicture.png
        :height: 10cm
        :align: center

    Pictured above: a helpful image to help you understand the content.

Save the file. Now, in the file **index.rst** (the one we were modifying earlier), add the following to the end of the file:

.. code-block:: RST
    
    Contents
    ----------

    .. toctree::

        myContent


The ``.. toctree::`` is a toctree directive. It creates a **T**\able **o**\f  **C**\ontents tree that connects **myContent.rst** to **index.rst**.

Congrats! You've created another page. Run ``make html`` and in your sidebar you should now be able to click on the page you just made and see its contents. 


Adding Questions
====================

In order to add questions, I recommend installing the example E-text and creating new pages off of that, as it will have the necessary functions that allow the 
questions to function. You could try installing all the dependencies into a fresh textbook, but there's no guarantee it will work.

MCQ's
-----------

Multiple choice questions can be created using the code like this:

.. code-block:: RST
    
    .. raw:: html


            <div class="mcq">
                    <p>What is my favorite color?:</p>
            <form name=Q1 id="Q1" data-component="binary">
            <input type="checkbox" id="Q1A1" value=""><label for="Q1A1">Red</label> <span id="Q1A1-feedback"> </span><br> 		
            <input type="checkbox" id="Q1A2" value="correct"><label for="Q1A2">Blue</label> <span id="Q1A2-feedback"> </span><br> 		
            <input type="checkbox" id="Q1A3" value=""><label for="Q1A3">Yellow</label> <span id="Q1A3-feedback"> </span><br> 		
            <input type="checkbox" id="Q1A4" value=""><label for="Q1A4">Green</label> <span id="Q1A4-feedback"> </span><br> 
                    <input type="button" value="Check" onclick="sendmcq('Q1')"><br>
            </form>
            <script id="Q1-answers" type="application/json"> 
            [ 	{ "ansid":"Q1A1", "answer": "Red", "feedback": "Wrong.", "result": ""  } ,	
            { "ansid":"Q1A2", "answer": "Blue", "feedback": "That's right!", "result": "correct"  } ,	
            { "ansid":"Q1A3", "answer": "Yellow", "feedback": "Incorrect.", "result": ""  } ,	
            { "ansid":"Q1A4", "answer": "Green", "feedback": "Wrong.", "result": ""  } 
        ]
        </script>

        </div>

If you put another question into your textbook, it's important to change any instances of "Q1" to "Q2", your next response will be "Q3", etc. 
We will now cover each component of an MCQ question. 

.. code-block:: RST

    <div class="mcq">
                    <p>What is my favorite color?:</p>

The above code declares that the question is a multiple choice question, and ``<p>What is my favorite color?:</p>`` is the question. 

.. code-block:: RST

     <form name=Q1 id="Q1" data-component="binary">
            <input type="checkbox" id="Q1A1" value=""><label for="Q1A1">Red</label> <span id="Q1A1-feedback"> </span><br> 		
            <input type="checkbox" id="Q1A2" value="correct"><label for="Q1A2">Blue</label> <span id="Q1A2-feedback"> </span><br> 		
            <input type="checkbox" id="Q1A3" value=""><label for="Q1A3">Yellow</label> <span id="Q1A3-feedback"> </span><br> 		
            <input type="checkbox" id="Q1A4" value=""><label for="Q1A4">Green</label> <span id="Q1A4-feedback"> </span><br> 

This portion sets up the four answer choices. ``<label for="Q1A1">Red</label>`` sets the text that the user sees. 

.. code-block:: RST

    <script id="Q1-answers" type="application/json"> 
            [ 	{ "ansid":"Q1A1", "answer": "Red", "feedback": "Wrong.", "result": ""  } ,	
            { "ansid":"Q1A2", "answer": "Blue", "feedback": "That's right!", "result": "correct"  } ,	
            { "ansid":"Q1A3", "answer": "Yellow", "feedback": "Incorrect.", "result": ""  } ,	
            { "ansid":"Q1A4", "answer": "Green", "feedback": "Wrong.", "result": ""  } 
        ]
        </script>

This sets up the feedback. You can change it to be whatever you'd like, such as giving the user hints to help arrive at the correct answer.



Fill in the blank
-------------------

Fill in the blank questions can take a variety of responses, and give feedback based on those responses.

.. code-block:: RST

    .. raw:: html

            <div class="fitb">
                    <p>38 + 24 is <input type="text" id="Q2" data-component="binary"> Don't forget to carry the one.</p>
                    <button type="button" onclick="sendfitb('Q2')">Check</button>
            <script id="Q2-answers" type="application/json">
            [

            { "regex": "62", "feedback": " Correct!", "result": "correct"  } ,
            { "regex": "52", "feedback": " Incorrect, don't forget to carry the one", "result": "incorrect"  } ,
            { "regex": "x", "feedback": " No. Try again.", "result": "incorrect"  }         ]
        </script>
        <p id="Q2-feedback"> <p>

        </div>

This uses similar principals as the multiple choice questions. Note how all instances of "Q1" have been replaced with "Q2".
The components of a fill in the blank question are similar as well.

.. code-block:: RST

    <div class="fitb">
                    <p>38 + 24 is <input type="text" id="Q2" data-component="binary"> Don't forget to carry the one.</p>
                    <button type="button" onclick="sendfitb('Q2')">Check</button>

The above code sets up the type of question, in this case **F**\ill **i**\n **t**\he **B**\lank. The ``<p>38 + 24 is <input type="text" id="Q2" data-component="binary"> Don't forget to carry the one.</p>`` 
will set up the question text that the user sees. The next part: ``<button type="button" onclick="sendfitb('Q2')">Check</button>`` will set up the text that the user sees on the check answer button.

.. code-block:: RST

    { "regex": "62", "feedback": " Correct!", "result": "correct"  } ,
    { "regex": "52", "feedback": " Incorrect, don't forget to carry the one", "result": "incorrect"  } ,
    { "regex": "x", "feedback": " No. Try again.", "result": "incorrect"  }    

This code will set up the answers to the question, as well as the feedback for each one. You can define specific answers and what their feedback will be. 
The code ``{ "regex": "x", "feedback": " No. Try again.", "result": "incorrect"  }`` will make it so that any answer besides "62" or "52" will give the feedback: " No. Try again."


Likert Questions
-------------------

A Likert scale is a rating scale that is used to gather opinions. Our Likert scale is on a scale of 1-5. 

You can create a Likert scale using the following code:

.. code-block:: RST
   
    .. raw:: html

    <div class="likert"><br>
    How confident are you that you understand geometry?
    <form id = "C3" data-component="binary">
        Not confident
    <input type="radio" name="C3" id="C3A1">
    <input type="radio" name="C3" id="C3A2">
    <input type="radio" name="C3" id="C3A3">
    <input type="radio" name="C3" id="C3A4">
    <input type="radio" name="C3" id="C3A5">
    Very confident
    <input type="button" value="Submit" onclick="sendlik('C3','binary')"><br>
    </form>
    </div>

This will create a Likert scale with 5 options, from "Not confident" to "Very confident."

.. code-block:: RST

    <div class="likert"><br>
    How confident are you that you understand geometry?
    <form id = "C3" data-component="binary">
        Not confident

The above code will set up the question type, in this case a Likert scale. It sets up the question text that the user sees. It will also set up the first (leftmost) option to "Not confident".

.. code-block:: RST

    <input type="radio" name="C3" id="C3A1">
    <input type="radio" name="C3" id="C3A2">
    <input type="radio" name="C3" id="C3A3">
    <input type="radio" name="C3" id="C3A4">
    <input type="radio" name="C3" id="C3A5">
    Very confident

The above code will set up the 5 buttons that the user can select. It will also set the last (rightmost) option to "Very confident."











.. toctree::

    

