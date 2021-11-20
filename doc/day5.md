Day 5:

今天要來看 05_optimized_surface_loading_and_soft_stretching.cpp
這支程式。

與 Day4 不同的地方是 loadSurface()，多了一個 optimizedSurface 存放“變形”過的圖檔資料。

我們在 71 行：

		gStretchedSurface = loadSurface( project_dir + "/res/stretch.bmp" );

傳入 "stretch.bmp" 這張圖，而 loadurface() 如何處理這張圖呢？

詳細過程如下：
首先，用 SDL_LoadBMP() 載入圖檔， loadedSurface 指標指向它，[API 說明](https://wiki.libsdl.org/SDL_LoadBMP)

	SDL_Surface* loadedSurface = SDL_LoadBMP( path.c_str() );

接着呼叫 SDL_ConvertSurface(): [API 說明](https://wiki.libsdl.org/SDL_ConvertSurface)

	optimizedSurface = SDL_ConvertSurface( loadedSurface, gScreenSurface->format, 0 );


第一個參數是前一步載入的圖檔的 surface，第二個參數是我們畫圖區的 format，第三個參數 SDL 還沒有用到，一律代入 0。

轉換什麼呢？舉例來說：
可能我們載入的圖檔，是 8-bit 着色 (rrrgggbb： 左邊 3-bit 代表紅色，中間 3-bit 代表綠色，右邊 2-bit 代表藍色)
但是畫圖區域可能是 24-bit ( rgb 各自 8-bit 共 24-bit )。
傳回的 optimizedSurface 會與 gScreenSurface 同樣格式，將來在每次畫圖的時候，可以直接畫到 gScreenSurface，
不需要在每次畫圖前再轉一次格式，省下了這個重複的動作。

經過 loadSurface() 後，我們畫圖的資料，就放在 gStretchedSurface 裡面，接着，呼叫

	SDL_BlitScaled( gStretchedSurface, NULL, gScreenSurface, &stretchRect );

進行"縮放畫圖"，也就是畫到 gScreenSurface 上面， 由 stretchRect 指示要縮放到的矩形位置，
（對！就是在 gScreenSurface 內畫上 gStretchedSuface 的內容，大小及位置由 stretchRect 規定）

好！ 今天的學習到這裡。讀者可以試試看，如何將 res/4-directions.bmp 畫到 windows  1/16 大小的右下角？

提示：

stretchRect 起始坐標從 (0,0) 改為 ( 寬度的 3/4, 長度的 3/4 )
大小為 寬度的 1/4, 長度的 1/4，原來的程式改為：

				stretchRect.x = SCREEN_WIDTH * 3/4;
				stretchRect.y = SCREEN_HEIGHT * 3/4;
				stretchRect.w = SCREEN_WIDTH/4;
				stretchRect.h = SCREEN_HEIGHT/4;

當然，載入圖檔記得改為 "res/4-directions.bmp"

你答對了嗎？
#
