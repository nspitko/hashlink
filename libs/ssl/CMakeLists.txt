if (WIN32)
    set(MBEDTLS_INCLUDE_DIRS ${INCLUDES_BASE_DIR}/mbedtls/include)
    set(MBEDTLS_LIBRARIES crypt32)

    add_library(ssl.hdll SHARED
        ${INCLUDES_BASE_DIR}/mbedtls/library/aes.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/aesni.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/arc4.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/asn1parse.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/asn1write.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/base64.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/bignum.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/blowfish.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/camellia.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ccm.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/certs.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/cipher.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/cipher_wrap.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ctr_drbg.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/debug.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/des.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/dhm.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ecdh.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ecdsa.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ecjpake.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ecp.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ecp_curves.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/entropy.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/entropy_poll.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/error.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/gcm.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/havege.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/hmac_drbg.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/md.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/md2.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/md4.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/md5.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/md_wrap.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/memory_buffer_alloc.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/oid.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/padlock.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/pem.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/pk.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/pkcs11.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/pkcs12.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/pkcs5.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/pkparse.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/pkwrite.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/pk_wrap.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/platform.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ripemd160.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/rsa.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/rsa_internal.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/sha1.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/sha256.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/sha512.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ssl_cache.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ssl_ciphersuites.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ssl_cli.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ssl_cookie.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ssl_srv.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ssl_ticket.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/ssl_tls.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/threading.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/timing.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/version.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/version_features.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/x509.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/x509write_crt.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/x509write_csr.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/x509_create.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/x509_crl.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/x509_crt.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/x509_csr.c
        ${INCLUDES_BASE_DIR}/mbedtls/library/xtea.c
        ssl.c
    )
else()
    find_package(MbedTLS REQUIRED)

    add_library(ssl.hdll SHARED
        ssl.c
    )
endif()

set_as_hdll(ssl)

target_include_directories(ssl.hdll
    PRIVATE
    ${MBEDTLS_INCLUDE_DIRS}
)

target_link_libraries(ssl.hdll
    libhl
    ${MBEDTLS_LIBRARIES}
)

if(APPLE)
	find_library(SECURITY_LIBRARY Security REQUIRED)
	find_library(COREFOUNDATION_LIBRARY CoreFoundation REQUIRED)
	target_link_libraries(ssl.hdll ${COREFOUNDATION_LIBRARY} ${SECURITY_LIBRARY})
endif()

install(
    TARGETS
        ssl.hdll
    DESTINATION ${HDLL_DESTINATION}
)
