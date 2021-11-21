Day 6: SDL Extension Library

如果我們在網絡上，搜尋到一張 PNG 圖檔，假設檔案名稱叫 loaded.png，
那麼，我們直接畫出來，可以嗎？

我們來試試看，把 loaded.png 放在 res 目錄下，然後
將昨天的 05_optimized_surface_loading_and_soft_stretching.cpp 稍微修改：

	gStretchedSurface = loadSurface( project_dir + "/res/loaded.png" );
	//gStretchedSurface = loadSurface( project_dir + "/res/stretch.bmp" );

可以 compile 但是不能 run，會這樣報錯：

	Unable to load image /home/tom/work/snake_tutorial/res/loaded.png! SDL Error: File is not a Windows BMP file
	Failed to load stretching image!
	Failed to load media!

原因就是 SDL Library __不能接收 BMP 以外的檔案格式__： 把 PNG 檔案餵給 SDL_LoadBMP() 就報出上面的錯誤。

SDL Library 有幾個衍生的 Library packages：

	libsdl2-image
	libsdl2-net
	libsdl2-mixer
	libsdl2-ttf

分別支援額外的 image、網路功能、混音器與字型。
需要安裝它們對應的 dev 版本才能使用:

	sudo apt install libsdl2-image-dev libsdl2-net-dev\
	libsdl2-mixer-dev libsdl2-ttf

在 libsdl2-image 安裝後，可以使用 IMG_Load() 載入——__其他格式__的圖檔。
今天的 06_extension_libraries_and_loading_other_image_formats.cpp
主要就是展示如何使用這個 API，首先用 IMG_Init() 初始化[使用說明在此](https://www.libsdl.org/projects/SDL_image/docs/SDL_image_8.html)。從使用說明的範例知道， reuturn 後，用 & flag 運算，可以確認 init image 有沒有成功。

所以，今天起，我們可以直接讀取 PNG, JPG, BMP, TIF 格式的圖檔來畫畫，方便許多。
#
