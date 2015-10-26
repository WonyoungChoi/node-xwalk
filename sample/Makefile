CC=gcc
libecho.so: echo_extension.c
	$(CC) -fPIC --shared -I../src -o $@ $^

clean:
	rm -f libecho.so