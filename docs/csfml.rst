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

Window
~~~~~~

Window managing
+++++++++++++++

.. c:type:: sfRenderWindow

This is the most basic element of any graphic program : a window.

.. c:function:: sfRenderWindow *sfRenderWindow_create(sfVideoMode mode, const char *title, sfUint32 style, const sfContextSettings *settings)

This function will setup a new window, based on the parameters given.

	**Parameters**

		:mode: :c:type:`sfVideoMode` - The video mode to use (width, height and bits per pixel of the window).

		:title: ``const char *`` - The title of the window.

		:style: :c:type:`sfUint32` - You must specify at least one of the following :

			* ``sfNone`` : None of the other style options will be used.
			* ``sfResize`` : Will add a "resize" button that will allow you to change the size of the window.
			* ``sfClose`` : Will add a "close" button that will allow you to close your window.
			* ``sfFullscreen`` : Will make your window full screen.

			If you want to add two or more parameters, simply separate them using pipes (see example below).

		:settings: :c:type:`sfContextSettings *` - Those are advanced render settings you can add. NULL to use default values.

	**Return**

		:c:type:`sfRenderWindow *` - New render window.

.. c:function:: sfRenderWindow *sfRenderWindow_createUnicode(sfVideoMode mode, const sfUint32 *title, sfUint32 style, const sfContextSettings *settings)

This function takes the same parameters as :c:func:`sfRenderWindow_create`, but this will take an array of integers as title, allowing you to have a unicode title.

.. code-block:: c

	sfVideoMode mode = {800, 600, 32};
	window = sfRenderWindow_create(mode, "My awesome window", sfResize | sfClose, NULL)

.. c:function:: void sfRenderWindow_destroy(sfRenderWindow *window)

This function allows you to destroy an existing window.

	**Parameter**

		:window: :c:type:`sfRenderWindow *` - The render window to destroy.

.. c:function:: void sfRenderWindow_close(sfRenderWindow *window)

This function will close a render window (while not destroying it's internal data.)

	**Parameter**

		:window: :c:type:`sfrenderWindow *` - The RenderWindow to close.

.. c:function:: void sfRenderWindow_clear(sfRenderWindow *window, sfColor color)

This function will clear all pixels in the window, replacing them with the given color.

	**Parameters**

		:window: :c:type:`sfRenderWindow *` - The RenderWindow to clear.

		:color: :c:type:`sfColor` - The color to which all pixel will change.

.. c:function:: void sfRenderWindow_display(sfRenderWindow *window)

This function will display all sprites on screen.

	**Parameter**

		:window: :c:type:`sfRenderWindow *` - The RenderWindow to display.

Getting window data
+++++++++++++++++++

There are a lot of data that can be obtained from a :c:type:`sfRenderWindow` object. As I don't want to spend my whole life writing this paragraph while knowing that no one will ever read this, I'll only list a bunch of useful functions. I'll probably add the others sooner or later.

.. c:function:: sfVector2u sfRenderWindow_getSize(const sfRenderWindow *window)

This function allows you to know the size of a given render window.

	**Return**

		:c:type:`sfVector2u` - Contains the size of the window in pixels.

.. c:function:: sfBool sfRenderWindow_hasFocus(sfRenderWindow *window)

	**Return**

		:c:type:`sfBool` - Will be ``sfTrue`` if the window has focus (i.e. it can receive inputs), ``sfFalse`` otherwise. This function can be useful if you want to pose your program if the window doesn't have focus.

.. c:function:: sfBool sfRenderWindow_isOpen(sfRenderWindow *window)

Tell whether or not a render window is open.

	**Return**

		:c:type:`sfBool` - Will be ``sfTrue`` if the window is open, ``sfFalse`` otherwise.

Other useful options
++++++++++++++++++++

Here are all other functions that I (Oursin) find useful for our first projects.

.. c:function:: sfBool sfRenderWindow_pollEvent(sfRenderWindow *window, sfEvent *event)

This function will check the event queue and pop one. As such, if you want your program to take all inputs into account, it is highly advised to empty the event queue at the start of your game loop.

	**Parameters**

		:window: :c:type:`sfRenderWindow *` - The target window.

		:event: :c:type:`sfEvent *` - This must take a pointer to the sfEvent object which will be filled.

	**Return**

		A :c:type:`sfBool` will be returned, indicating whether or not an event was returned.

.. c:function:: void sfRenderWindow_setFramerateLimit(sfRenderWindow *window, unsigned int limit)

This function allows you to set your program's framerate.

	**Parameters**

		:window: :c:type:`sfRenderWindow` - Target render window.

		:limit: ``unsigned int`` - Defines the maximum number of frames your program should display each second.

.. c:function:: void sfRenderWindow_setKeyRepeatEnabled(sfRenderWindow *window, sfBool enabled)

This function enables or disables automatic key-repeat for keydown events. If enabled, a key pressed down will create new :c:type:`sfEvent` objects all time until it is released.

	**Parameters**

		:window: :c:type:`sfRenderWindow` - Target render window.

		:enabled: :c:type:`sfBool` - ``sfTrue`` to enable, ``sfFalse`` to disable.

.. c:function:: void sfRenderWindow_setMouseCursorVisible(sfRenderWindow *window, sfBool show)

This function allows you to hide the mouse cursor on a render window.

	**Parameters**
		:window: :c:type:`sfRenderWindow` - Target render window.

		:show: :c:type:`sfBool` - ``sfTrue`` to show, ``sfFalse`` to hide.

Drawing
~~~~~~~

In CSFML, there are 4 types of objects that can be displayed, 3 of them beign ready to be used : sprites, text and shapes. The other one, vertex arrays, is designed to help you create your own drawable entities, but you would probably not use it for now.

.. c:function:: void sfRenderWindow_drawSprite(sfRenderWindow *window, const sfSprite *sprite, sfRenderStates *states)

	**Parameters**

		:window: :c:type:`sfRenderWindow *` - The window to draw to.

		:sprite: :c:type:`sfSprite *` - The sprite to draw.

		:states: :c:type:`sfRenderStates *` - This can be used to use advanced render options, such as shaders, transfomations etc...

.. c:function:: void sfRenderWindow_drawText(sfRenderWindow *window, sfText *text, sfRenderStates *states)

	**Parameters**

		:window: :c:type:`sfRenderWindow *` - The window to draw to.

		:sprite: :c:type:`sfText *` - The text object to display on screen. Note that this is not a char *, but an sfText object, which must be created first.

		:states: :c:type:`sfRenderStates *` - This can be used to use advanced render options, such as shaders, transfomations etc...

.. c:function:: void sfRenderWindow_drawShape(sfRenderWindow *window, sfShape *shape, sfRenderStates *states)

	**Parameters**

		:window: :c:type:`sfRenderWindow *` - The window to draw to.

		:sprite: :c:type:`sfShape *` - The shape object to display on screen.

		:states: :c:type:`sfRenderStates *` - This can be used to use advanced render options, such as shaders, transfomations etc...

Examples
--------

RenderWindow
~~~~~~~~~~~~

Here is a sample example of most functions related to :c:type:`sfRenderWindow`. For the following example, we will assume that ``sprite``, ``text`` and ``shape`` were already created, and are variables of type :c:type:`sfSprite`, :c:type:`sfText` and :c:type:`sfShape`, respectively.

We will also assume that ``event`` is of type ``sfEvent``.

.. code-block:: c

	sfVideoMode mode = {1080, 720};
	sfRenderWindow *window;

	window = sfRenderWindow_create(mode, "window", sfClose, NULL);
	sfRenderWindow_setFramerateLimit(window, 60);
	while (sfRenderWindow_isOpen(window) && sfRenderWindow_hasFocus
						(window)) {
		while (sfRenderWindow_pollEvent(window, &event)) {
			if (event.type == sfEvtClosed)
				sfRenderWindow_close(window);
		}
		sfRenderWindow_clear(window, sfBlack);
		sfRenderWindow_drawSprite(window, sprite, NULL);
		sfRenderWindow_drawShape(window, shape, NULL);
		sfRenderWindow_drawText(window, text, NULL);
		sfRenderWindow_display(window);
	}
	sfRenderWindow_destroy(window);
