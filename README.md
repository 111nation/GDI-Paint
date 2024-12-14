<h1><u>GDI Paint Library Wrapper</u></h1>

The paint wrapper is semi-inspired by CSS. 

<h3>Required:</h3>

- C++
- Ensure you have the ``` window.h``` header present with your compiler.
- Ensure ```string``` header present.
- Compile with `-lUser32 -lmsimg32 -windows` libraries to ensure full functionality.

<h2>Documentation</h2>

<h1>Creating a square/ rectangle</h1>

Let's create a simple rectangle. Assuming that you've already made a window that can be painted. Pass the window's <b>Device in context</b> or its `hDC` to `paint.area`. 
Remember to clear the `paint.area` depending on how you assigned the `hDC`.

```cpp
  #include <windows.h>
  #include "PaintLib/paint.h" // NB

  ...

  LRESULT CALLBACK WindowProc(HWND hWnd, UINT event, WPARAM wParam, LPARAM lParam) {
    ...
      case WM_PAINT: { 
        // Use PaintLib when the window needs to be painted again
        Paint paint;
        paint.area = GetDC(hWnd); // Pass window's paintable area

        // All painting goes between here

        ReleaseDC(hWnd, paint.area); // Remember to clear the area!
        break;
      }
    ...
  }

  ...
```

Now let's make a simple red square! With a solid black border!

```cpp
  Paint paint;
  paint.area = GetDC(hWnd);

  // Sizing
  paint.x = 100;
  paint.xend = 200;
  paint.y = 150;
  paint.yend = 250;
  // Other styling
  paint.color = RGB(255, 0, 0);
  paint.border.width = 1;
  paint.border.color = RGB(0, 0, 0);
  // 'Commit' your square
  paint.Rectangle();

  ReleaseDC(hWnd, paint.area);
```

You can even make the red square rounded!
```cpp
  paint.RoundRect(25); // 25px corner rounding
  paint.RoundRect(25, 15); // Unequal corner rounding
```

<h2>Creating a circle/ ellipse</h2>

Repeat the process of assigning the <b>the device in context</b> of the window. This time use ```paint.Circle()```. Use ```paint.Reset()``` to reset most common styling to default.
If the coordinates are not equal a ellipse is formed.

```cpp
  paint.Reset();
  const int DIAMETER = 50;

  paint.x = 100;
  paint.xend = paint.x + DIAMETER;
  paint.y = 100;
  paint.yend = paint.y + DIAMETER;
  paint.color = RGB(255, 255, 0);
  paint.Circle();
```

<h2>Creating a line</h2>

Reset the library and this time, creating a line involves manipulating the styling of the border. 

```cpp
  paint.Reset();

  // Placement
  paint.x = 150;
  paint.xend = 205;
  paint.y = 10;
  paint.yend = 15;
  // Styling
  paint.border.color = RGB(255, 255, 255);
  paint.border.width = 3;
  paint.border.style = PS_DASH;
  paint.Line();
```

<h2>Creating some text!</h2>

Not only can the library make shapes, but the text is supported too! Custom fonts are supported!
The process of creating text involves making an instantiation of the font library that is included too. Then define your font,and  font properties, then call ```paint.Text()```

```cpp
  // Adjust font
  Font Montserrat;
  Montserrat.name = "Montserrat"; // Assuming you have the Montserrat font pack installed otherwise default font used
  Montserrat.size = 14;  // Adjust height of font
  Montserrat.width = 700;
  Montserrat.underline = TRUE; // DWORD from windows.h
  Montserrat.italics = TRUE;

  // Adjust the text placement
```
