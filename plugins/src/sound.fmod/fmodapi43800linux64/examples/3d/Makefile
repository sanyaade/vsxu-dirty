3d_cpp:	main.cpp
	g++ -O3 -o 3d main.cpp ../../api/lib/libfmodex64.so -pthread

3d_c:	main.c
	g++ -O3 -o 3d main.c ../../api/lib/libfmodex64.so -pthread

run:
	./3d

clean:
	rm -f 3d
