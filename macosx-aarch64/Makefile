CC = cc
SRC = alpn.c clientcert.c options.c session.c ssl.c threads.c util.c
OBJ = $(patsubst %.c, target/%.o, $(SRC))

default: target/classes/macosx-aarch64/libwfssl.dylib

clean:
	rm -rf target

target/classes/macosx-aarch64:
	mkdir -p target/classes/macosx-aarch64

target/%.o : ../libwfssl/src/%.c target/classes/macosx-aarch64
	$(CC) $(CFLAGS) -mmacosx-version-min=11.0 -target arm64-apple-macos11.0 -Werror -Wall -Wmissing-prototypes -Wstrict-prototypes -Wmissing-declarations -Wpointer-arith -std=c89 -Wdeclaration-after-statement -Wformat -Wformat-security -Wunused -Wno-unknown-pragmas -c $< -o $@ -I../libwfssl/include -I${JAVA_HOME}/include -I${JAVA_HOME}/include/darwin

target/classes/macosx-aarch64/libwfssl.dylib: $(OBJ)
	$(CC) $(CFLAGS) -mmacosx-version-min=11.0 -target arm64-apple-macos11.0 -dynamiclib $(OBJ) -o $@ $(LDFLAGS)
