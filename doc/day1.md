Day 1
我採用 cmake，參考了幾個簡單範例之後，歸納如下：
1.Compile 需要 /usr/include/SDL2， CMake 支援 SDL2 環境，因此我們在 CMakeLists.txt 裡面加上：

	# Find the SDL2 library
	find_package(SDL2 REQUIRED)

那麼我就可以告訴 compiler，header 檔案與 library 檔案的位置，分別是：

    #Add SDL2 into include directories
	include_directories(${SDL2_INCLUDE_DIR})

    # And in the linking
	target_link_libraries(${PROJECT_NAME} ${SDL2_LIBRARIES})

這裡的 ${PROJECT_NAME} 就是 snake --- 在第二行指定了.
${SDL2_LIBRARIES} 與 ${SDL2_INCLUDE_DIR} 都是 find_package(SDL2) 發現的


看到 01_hello_SDL.cpp 可以觀察 啟用 SDL 的順序是：

	SDL_Init()
	SDL_CreateWindow()

	用 SDL 畫圖 ...

	畫畫動作後，強迫更新畫面，讓前面畫的圖出現
	SDL_UpdateWindowSurface()

	SDL_DestroyWindow()
	SDL_Quit()


#
