Day 4:

今天我們來看 Lesson 4: [key press](ohttps://lazyfoo.net/tutorials/SDL/04_key_presses/index.pph)

如圖：
![array of images](https://user-images.githubusercontent.com/252880/142641345-7fe3d0e3-d815-46bd-b4dd-68e26c8d1648.png)

我們在 loadMedia() 載入 5 張圖，放在一個陣列裡面，每個元素是一個 SDL_Surface* 分別是起始圖、上、下、左、右 按下後要顯示的圖。
這裡隱含一個 c 語言的語法：enum 如果沒有特別指定的話，就內定為從 0 開始，依序給值。
也就是

	enum KeyPressSurfaces
	{
		KEY_PRESS_SURFACE_DEFAULT,   // 0
		KEY_PRESS_SURFACE_UP,        // 1
		KEY_PRESS_SURFACE_DOWN,      // 2
		KEY_PRESS_SURFACE_LEFT,      // 3
		KEY_PRESS_SURFACE_RIGHT,     // 4
		KEY_PRESS_SURFACE_TOTAL      // 5
	};

的意思，這樣

	//The images that correspond to a keypress
	SDL_Surface* gKeyPressSurfaces[ KEY_PRESS_SURFACE_TOTAL ];

就是一個存放 5 個指標的陣列，每一個指標，都是一個 SDL_Surface* 指標，它指到圖檔載入的記憶體位置，
所以，如果偵測到 up 按鍵，就把 gKeyPressSurface[KEY_PRESS_SURFECE_UP] 畫到 gScreenSruface 上面，

用什麼 API 來畫呢？ 第 224 行的:

				//Apply the current image
				SDL_BlitSurface( gCurrentSurface, NULL, gScreenSurface, NULL );

處理其他的按鍵，也是一樣，只是找出不同的 SDL_Surface* 來畫而已。
