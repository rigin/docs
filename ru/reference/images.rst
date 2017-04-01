Изображения
===========

:doc:`Phalcon\\Image <../api/Phalcon_Image>` - это компонент, который позволяет вам манипулировать файлами изображений. Несколько операций могут выполняться над одним и тем же объектом изображения.

.. highlights::

    Это руководство не предназначено для полной документации о доступных методах и их аргументах. 
    Пожалуйста, посетите :doc:`API <../api/index>` для получения полной справки.

Адаптеры
--------
Этот компонент использует адаптеры для инкапсуляции определенных программ манипулятора изображения. 
Поддерживаются следующие программы обработки изображений:

+--------------------------------------------------------------------------------+--------------------------------------------+
| Класс                                                                          | Описание                                   |
+================================================================================+============================================+
| :doc:`Phalcon\\Image\\Adapter\\Gd <../api/Phalcon_Image_Adapter_Gd>`           | Требует      `GD PHP extension`_.          |
+--------------------------------------------------------------------------------+--------------------------------------------+
| :doc:`Phalcon\\Image\\Adapter\\Imagick <../api/Phalcon_Image_Adapter_Imagick>` | Требует      `ImageMagick PHP extension`_. |
+--------------------------------------------------------------------------------+--------------------------------------------+

Реализация собственных адаптеров
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Для создания собственных графических адаптеров или расширения существующих интерфейсов должен быть реализован интерфейс :doc:`Phalcon\\Image\\AdapterInterface <../api/Phalcon_Image_AdapterInterface>`.

Сохранение и рендеринг изображений
----------------------------------
Прежде чем мы начнем с различных функций компонента изображения, стоит понять, как сохранять и отображать эти изображения.

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    // ...

    // Перезаписать исходное изображение
    $image->save();

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    // ...

    // Сохранить 'new-image.jpg'
    $image->save("new-image.jpg");

Вы также можете изменить формат изображения:

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    // ...

    // Сохранить как файл PNG
    $image->save("image.png");

При сохранении в формате JPEG вы также можете указать качество как второй параметр:

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    // ...

    // Сохранить как JPEG с качеством 80%
    $image->save("image.jpg", 80);

Изменение размеров изображений
------------------------------
Существует несколько способов изменения размера:

- :code:`\Phalcon\Image::WIDTH`
- :code:`\Phalcon\Image::HEIGHT`
- :code:`\Phalcon\Image::NONE`
- :code:`\Phalcon\Image::TENSILE`
- :code:`\Phalcon\Image::AUTO`
- :code:`\Phalcon\Image::INVERSE`
- :code:`\Phalcon\Image::PRECISE`

:code:`\Phalcon\Image::WIDTH`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Высота будет генерироваться автоматически, чтобы сохранить пропорции одинаковыми; 
Если вы укажете высоту, она будет проигнорирована.

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $image->resize(
        300,
        null,
        \Phalcon\Image::WIDTH
    );

    $image->save("resized-image.jpg");

:code:`\Phalcon\Image::HEIGHT`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Ширина будет генерироваться автоматически, чтобы сохранить пропорции одинаковыми; 
Если вы укажете ширину, она будет проигнорирована.

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $image->resize(
        null,
        300,
        \Phalcon\Image::HEIGHT
    );

    $image->save("resized-image.jpg");

:code:`\Phalcon\Image::NONE`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Константа :code:`NONE` игнорирует соотношение исходного изображения. Ширина и высота не требуются. Если размер не указан, будет использовано исходное измерение. Если новые пропорции отличаются от исходных пропорций, изображение может быть искажено и растянуто.

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $image->resize(
        400,
        200,
        \Phalcon\Image::NONE
    );

    $image->save("resized-image.jpg");

:code:`\Phalcon\Image::TENSILE`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Подобно константе :code:`NONE`, константа :code:`TENSILE` игнорирует соотношение исходного изображения. 
Необходимо указать ширину и высоту. Если новые пропорции отличаются от исходных пропорций, 
изображение может быть искажено и растянуто.

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $image->resize(
        400,
        200,
        \Phalcon\Image::NONE
    );

    $image->save("resized-image.jpg");

Обрезание изображений
---------------------
Например, чтобы получить квадрат 100 пикселей на 100 пикселей от центра изображения:

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $width   = 100;
    $height  = 100;
    $offsetX = (($image->getWidth() - $width) / 2);
    $offsetY = (($image->getHeight() - $height) / 2);

    $image->crop($width, $height, $offsetX, $offsetY);

    $image->save("cropped-image.jpg");

Вращение изображений
--------------------
.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    // Rotate an image by 90 degrees clockwise
    $image->rotate(90);

    $image->save("rotated-image.jpg");

Переворачивание изображений
---------------------------
Вы можете перевернуть изображение по горизонтали (используя константу :code:`\Phalcon\Image::HORIZONTAL`) и вертикально (используя константу :code:`Phalcon\Image::VERTICAL`):

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    // Flip an image horizontally
    $image->flip(
        \Phalcon\Image::HORIZONTAL
    );

    $image->save("flipped-image.jpg");

Увеличение резкозти изображений
-------------------------------
Метод :code:`sharpen()` принимает один параметр - целое число от 0 (без эффекта) до 100 (очень резкое):

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $image->sharpen(50);

    $image->save("sharpened-image.jpg");

Добавление водяных знаков к изображениям
----------------------------------------

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $watermark = new \Phalcon\Image\Adapter\Gd("me.jpg");

    // Поместите водяной знак в верхний левый угол
    $offsetX = 10;
    $offsetY = 10;

    $opacity = 70;

    $image->watermark(
        $watermark,
        $offsetX,
        $offsetY,
        $opacity
    );

    $image->save("watermarked-image.jpg");

Конечно, вы можете также манипулировать водяным знаком, прежде чем применить его к основному изображению:

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $watermark = new \Phalcon\Image\Adapter\Gd("me.jpg");

    $watermark->resize(100, 100);
    $watermark->rotate(90);
    $watermark->sharpen(5);

    // Поместите водяной знак в нижний правый угол с отступом 10 пикселей
    $offsetX = ($image->getWidth() - $watermark->getWidth() - 10);
    $offsetY = ($image->getHeight() - $watermark->getHeight() - 10);

    $opacity = 70;

    $image->watermark(
        $watermark,
        $offsetX,
        $offsetY,
        $opacity
    );

    $image->save("watermarked-image.jpg");

Размытие изображений
--------------------
Метод :code:`blur()` принимает единственный параметр - целое число от 0 (без эффекта) до 100 (очень размытое):

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $image->blur(50);

    $image->save("blurred-image.jpg");

Пикселирование изображений
--------------------------
Метод :code:`pixelate()` принимает единственный параметр - чем выше целое число, тем больше пикселизация изображения становится:

.. code-block:: php

    <?php

    $image = new \Phalcon\Image\Adapter\Gd("image.jpg");

    $image->pixelate(10);

    $image->save("pixelated-image.jpg");

.. _`GD PHP extension`: http://php.net/manual/en/book.image.php
.. _`ImageMagick PHP extension`: http://php.net/manual/en/book.imagick.php
