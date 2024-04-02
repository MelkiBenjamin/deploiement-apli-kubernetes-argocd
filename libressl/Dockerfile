FROM busybox:1.36.1-glibc

# Installation de LibreSSL
RUN mkdir -p /usr/local/lib
RUN wget https://ftp.openbsd.org/pub/OpenBSD/LibreSSL/libressl-3.9.1.tar.gz
RUN tar -xzvf libressl-3.9.1.tar.gz
RUN cd libressl-3.9.1 && \
    ./configure --enable-static && \
    make && \
    make install && \
    cd .. && \
    rm -rf libressl-3.9.1 libressl-3.9.1.tar.gz

# Copie des bibliothèques de LibreSSL dans le répertoire approprié
RUN cp /usr/local/lib/libtls.so.24 /usr/local/lib/libssl.so.48 /usr/local/lib/libcrypto.so.51 /lib/

# Copie de l'exécutable de l'application depuis l'étape de compilation
#COPY --from=builder /usr/src/app/target/release/your_binary /usr/local/bin/