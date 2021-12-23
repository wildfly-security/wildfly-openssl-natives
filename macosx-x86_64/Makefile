CC = cc
SRC = alpn.c clientcert.c options.c session.c ssl.c threads.c util.c
OBJ = $(patsubst %.c, target/%.o, $(SRC))

default: target/classes/macosx-x86_64/libwfssl.dylib

clean:
	rm -rf target

target/classes/macosx-x86_64:
	mkdir -p target/classes/macosx-x86_64

target/%.o : ../libwfssl/src/%.c target/classes/macosx-x86_64
	$(CC) $(CFLAGS) -arch x86_64 -Werror -Wall -Wmissing-prototypes -Wstrict-prototypes -Wmissing-declarations -Wpointer-arith -std=c89 -Wdeclaration-after-statement -Wformat -Wformat-security -Wunused -Wno-unknown-pragmas -c $< -o $@ -I../libwfssl/include -I${JAVA_HOME}/include -I${JAVA_HOME}/include/darwin

target/classes/macosx-x86_64/libwfssl.dylib: $(OBJ)
	$(CC) $(CFLAGS) -arch x86_64 -dynamiclib $(OBJ) -o $@ $(LDFLAGS)
