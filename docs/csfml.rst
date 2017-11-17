.. EPITECH 2022 - Technical Documentation documentation master file, created by
   sphinx-quickstart on Tue Nov  7 09:05:01 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

CSFML : Graphical Programming
=============================

Table of contents
-----------------

.. toctree::
   :maxdepth: 2

Just to be sure that you still know the basics.

Reference
---------

Window managing
~~~~~~~~~~~~~~~

.. c:type:: sfRenderWindow

This is the most basic element of any graphic program : a window.

.. c:function:: sfRenderWindow *sfRenderWindow_create(sfVideoMode mode, const char *title, sfUint32 style, const sfContextSettings *settings)

This function will setup a new window, based on the parameters given.

:param mode: :c:type:`sfVideoMode` : The video mode to use (width, height and bits per pixel of the window).

:param title: ``const char *`` : The title of the window.

:param style: :c:type:`sfUint32` : You must specify at least one of the following :

	* sfNone : None of the other style options will be used.
	* sfResize : Will add a "resize" button that will allow you to change the size of the window.
	* sfClose : Will add a "close" button that will allow you to close your window.
	* sfFullscreen : Will make your window full screen.

	If you want to add two or more parameters, simply separate them using pipes (see example below).

:param settings: :c::type:`sfContextSettings` * : Those are advanced render settings you can add. NULL to use default values.

.. c:function:: sfRenderWindow *sfRenderWindow_createUnicode(sfVideoMode mode, const sfUint32 *title, sfUint32 style, const sfContextSettings *settings)

This function takes the same parameters as :c:func:`sfRenderWindow_create`, but this will take an array of integers as title, allowing you to have a unicode title.

.. code-block:: c

	sfVideoMode mode = {800, 600, 32};
	window = sfRenderWindow_create(mode, "My awesome window", sfResize | sfClose, NULL)

.. c:function:: void sfRenderWindow_destroy(sfRenderWindow *window)

This function allows you to destroy an existing window.

:param window: ``sfRenderWindow *`` : The RenderWindow to destroy.

.. c:function:: void sfRenderWindow_close(sfRenderWindow *window)

This function will close a render window (while not destroying it's internal data.)

:param window: ``sfrenderWindow *`` : The RenderWindow to close.

.. c:function:: void sfRenderWindow_clear(sfRenderWindow *window, sfColor color)

This function will clear all pixels in the window, replacing them with the given color.

:param window: ``sfRenderWindow *`` : The RenderWindow to clear.

:param color: :c:type:`sfColor` : The color to which all pixel will change.

.. c:function:: void sfRenderWindow_display(sfRenderWindow *window)

This function will display all sprites on screen.

:param window: ``sfRenderWindow *`` : The RenderWindow to display.

Drawing
~~~~~~~

In CSFML, there are 4 types of objects that can be displayed, 3 of them beign ready to be used : sprites, text and shapes. The other one, vertex arrays, is designed to help you create your own drawable entities, but you would probably not use it for now.

.. c:function:: void sfRenderWindow_drawSprite(sfRenderWindow *window, const sfSprite *sprite, sfRenderStates *states)

:param window: ``sfRenderWindow *`` : The window to draw to.

:param sprite: ``sfSprite *`` : The sprite to draw.

:param states: ``sfRenderStates *`` : This can be used to use advanced render options, such as shaders, transfomations etc...

.. c:function:: void sfRenderWindow_drawText(sfRenderWindow *window, sfText *text, sfRenderStates *states)

:param window: ``sfRenderWindow *`` : The window to draw to.

:param sprite: ``sfSprite *`` : The text object to display on screen. Note that this is not a char *, but an sfText object, which must be created first.

:param states: ``sfRenderStates *`` : This can be used to use advanced render options, such as shaders, transfomations etc...
