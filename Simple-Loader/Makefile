#invoke make inside following directories and in this order: loader, launch, fib
#move the lib_simpleloader.so and launch binaries inside bin directory
#Provide the command for cleanup

all:
	$(MAKE) -C loader
	$(MAKE) -C launch
	$(MAKE) -C test

change:
	$(MAKE) -C test
	
clean:
	$(MAKE) -C loader clean
	$(MAKE) -C launch clean
	$(MAKE) -C test clean
	rm -f bin/*
