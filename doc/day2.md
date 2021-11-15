# Day2

# 構想

昨天匆忙介紹 SDL 初始化的部分，今天要根據[這裡](https://lazyfoo.net/tutorials/SDL/02_getting_an_image_on_the_screen/index.php)
的內容，開始在上面畫圖。

整個 Tutorial 構想是：前面照着 SDL tutorial 每天的教程摸索 SDL 如何使用，大概到夠用的程度後。再開始編寫我們的 snake 主程式。

把畫圖的動作獨立出來，將來可以切換到其他不同於 SDL 的 library 畫圖。在設計程式的時候，有時候會像這樣多設計一層，保留將來修改的彈性。 當然，這是專案開發常用的做法，如果是學校作業的等級，那就不必要這麼費工。

# 今天的內容

就照着 lesson 2 的部分學習新的 SDL 用法。

在 CMakeList.txt 新增：

	# Define Project directory
	add_compile_definitions("PROJECT_DIR=\"${PROJECT_SOURCE_DIR}\"")

這樣 program 裡面就可以知道現在的 project 目錄位置，我們的 bitmap 檔案、以後的畫圖資源，都要靠這個定義 當作參考。

在 loadMedia() 新增

	std::string project_dir = std::string(PROJECT_DIR);
	std::string bmp_file = project_dir + "/res/hello_world.bmp";

	//Load splash image
	gHelloWorld = SDL_LoadBMP( bmp_file.c_str() );

其中 PROJECT_DIR 是 CMake 傳給我們的，這樣我們的目錄如果有搬動，只要重新 cmake 一遍就可以更新路徑。程式裡面尋找 bitmap 檔案就不會出錯，也不需要手動再修改。

bmp_file.c_str() 只是因為 SDL_LoadBMP() 需要餵 c 的 (char *) 進去。這裡是把 c++ string 轉換成 c string 的動作。

最後檢查是否在 res/ 下放了 hello_world.bmp 檔案，就可以

	cmake .
	make
	./snake

OK, 程式編譯環境介紹到此。

# Program Tutorial

我們採用三個 global 變數，都是指標，分別存放 Window 資料、繪圖區域、以及 bitmap 圖檔資料：

	//The window we'll be rendering to
	SDL_Window* gWindow = NULL;

	//The surface contained by the window
	SDL_Surface* gScreenSurface = NULL;

	//The image we will load and show on the screen
	SDL_Surface* gHelloWorld = NULL;

Main 程式現在分成小 functions 完成，它們清楚的表示 初始、畫圖、結束 三個階段.
