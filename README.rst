Introduction
============


.. image:: https://readthedocs.org/projects/adafruit-circuitpython-displayio_sh1106/badge/?version=latest
    :target: https://docs.circuitpython.org/projects/displayio_sh1106/en/latest/
    :alt: Documentation Status


.. image:: https://img.shields.io/discord/327254708534116352.svg
    :target: https://adafru.it/discord
    :alt: Discord


.. image:: https://github.com/adafruit/Adafruit_CircuitPython_DisplayIO_SH1106/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_DisplayIO_SH1106/actions
    :alt: Build Status


.. image:: https://img.shields.io/badge/code%20style-black-000000.svg
    :target: https://github.com/psf/black
    :alt: Code Style: Black

DisplayIO compatible library for SH1106 OLED displays


Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://circuitpython.org/libraries>`_
or individual libraries can be installed using
`circup <https://github.com/adafruit/circup>`_.

Installing from PyPI
=====================
On supported GNU/Linux systems like the Raspberry Pi, you can install the driver locally `from
PyPI <https://pypi.org/project/adafruit-circuitpython-displayio_sh1106/>`_.
To install for current user:

.. code-block:: shell

    pip3 install adafruit-circuitpython-displayio-sh1106

To install system-wide (this may be required in some cases):

.. code-block:: shell

    sudo pip3 install adafruit-circuitpython-displayio-sh1106

To install in a virtual environment in your current project:

.. code-block:: shell

    mkdir project-name && cd project-name
    python3 -m venv .env
    source .env/bin/activate
    pip3 install adafruit-circuitpython-displayio-sh1106



Usage Example
=============

.. code-block:: python3

    import busio
    import displayio
    import terminalio
    from adafruit_display_text import label
    import adafruit_displayio_sh1106

    displayio.release_displays()

    spi = busio.SPI(board.SCK, board.MOSI)
    display_bus = displayio.FourWire(
        spi,
        command=board.OLED_DC,
        chip_select=board.OLED_CS,
        reset=board.OLED_RESET,
        baudrate=1000000,
    )

    WIDTH = 128
    HEIGHT = 64
    BORDER = 5
    display = adafruit_displayio_sh1106.SH1106(display_bus, width=WIDTH, height=HEIGHT)

    # Make the display context
    splash = displayio.Group()
    display.show(splash)

    color_bitmap = displayio.Bitmap(WIDTH, HEIGHT, 1)
    color_palette = displayio.Palette(1)
    color_palette[0] = 0xFFFFFF  # White

    bg_sprite = displayio.TileGrid(color_bitmap, pixel_shader=color_palette, x=0, y=0)
    splash.append(bg_sprite)

    # Draw a smaller inner rectangle
    inner_bitmap = displayio.Bitmap(WIDTH - BORDER * 2, HEIGHT - BORDER * 2, 1)
    inner_palette = displayio.Palette(1)
    inner_palette[0] = 0x000000  # Black
    inner_sprite = displayio.TileGrid(
        inner_bitmap, pixel_shader=inner_palette, x=BORDER, y=BORDER
    )
    splash.append(inner_sprite)

    # Draw a label
    text = "Hello World!"
    text_area = label.Label(
        terminalio.FONT, text=text, color=0xFFFFFF, x=28, y=HEIGHT // 2 - 1
    )
    splash.append(text_area)

    while True:
        pass

Documentation
=============

API documentation for this library can be found on `Read the Docs <https://docs.circuitpython.org/projects/displayio_sh1106/en/latest/>`_.

Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_DisplayIO_SH1106/blob/HEAD/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.

Documentation
=============

For information on building library documentation, please check out
`this guide <https://learn.adafruit.com/creating-and-sharing-a-circuitpython-library/sharing-our-docs-on-readthedocs#sphinx-5-1>`_.
