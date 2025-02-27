cmake_minimum_required(VERSION 3.25)
project(spam)

#╭──────────────────────────────────────╮
#│               pd.cmake               │
#╰──────────────────────────────────────╯
cmake_policy(SET CMP0135 NEW)
set(PDCMAKE_FILE ${CMAKE_BINARY_DIR}/pd.cmake)
if(NOT EXISTS ${PDCMAKE_FILE})
    message(STATUS "Downloading pd.cmake")
    file(
        DOWNLOAD https://raw.githubusercontent.com/pure-data/pd.cmake/refs/tags/v0.1.0/pd.cmake ${PDCMAKE_FILE}
        SHOW_PROGRESS
        STATUS DOWNLOAD_STATUS)
endif()
include(${PDCMAKE_FILE})


#╭──────────────────────────────────────╮
#│               Sources                │
#╰──────────────────────────────────────╯
if(CMAKE_C_COMPILER_ID STREQUAL "MSVC")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /W4")
elseif(CMAKE_C_COMPILER_ID STREQUAL "GNU")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -pedantic")
elseif(CMAKE_C_COMPILER_ID STREQUAL "Clang")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Weverything -Wno-padded -Wno-reserved-id-macro -Wno-disabled-macro-expansion -Wno-missing-variable-declarations")
endif()

set(SPAM_SRC_DIR "${PROJECT_SOURCE_DIR}/src/")

file(GLOB SPAMTILDESRC
  ${SPAM_SRC_DIR}/spam.tie.h ${SPAM_SRC_DIR}/spam.tie.c
  ${SPAM_SRC_DIR}/spam.utils.h ${SPAM_SRC_DIR}/spam.utils.c ${SPAM_SRC_DIR}/spam_tilde.c)
source_group(src FILES ${SPAMTILDESRC})
pd_add_external(spam~ "${SPAMTILDESRC}")

file(GLOB SPAMCURRENTSRC
  ${SPAM_SRC_DIR}/spam.tie.h ${SPAM_SRC_DIR}/spam.tie.c
  ${SPAM_SRC_DIR}/spam.io.h ${SPAM_SRC_DIR}/spam.io.c ${SPAM_SRC_DIR}/spam.in.c)
source_group(src FILES ${SPAMCURRENTSRC})
pd_add_external(spam.in "${SPAMCURRENTSRC}")
#
file(GLOB SPAMCURRENTSRC
  ${SPAM_SRC_DIR}/spam.tie.h ${SPAM_SRC_DIR}/spam.tie.c
  ${SPAM_SRC_DIR}/spam.io.h ${SPAM_SRC_DIR}/spam.io.c ${SPAM_SRC_DIR}/spam.out.c)
source_group(src FILES ${SPAMCURRENTSRC})
pd_add_external(spam.out "${SPAMCURRENTSRC}")
#
file(GLOB SPAMCURRENTSRC
    ${SPAM_SRC_DIR}/spam.tie.h ${SPAM_SRC_DIR}/spam.tie.c
    ${SPAM_SRC_DIR}/spam.io.h ${SPAM_SRC_DIR}/spam.io.c ${SPAM_SRC_DIR}/spam.in_tilde.c)
source_group(src FILES ${SPAMCURRENTSRC})
pd_add_external(spam.in~ "${SPAMCURRENTSRC}")
#
file(GLOB SPAMCURRENTSRC
    ${SPAM_SRC_DIR}/spam.tie.h ${SPAM_SRC_DIR}/spam.tie.c
    ${SPAM_SRC_DIR}/spam.io.h ${SPAM_SRC_DIR}/spam.io.c ${SPAM_SRC_DIR}/spam.out_tilde.c)
source_group(src FILES ${SPAMCURRENTSRC})
pd_add_external(spam.out~ "${SPAMCURRENTSRC}")
#
file(GLOB SPAMCURRENTSR ${SPAM_SRC_DIR}/spam.connect.c)
source_group(src FILES ${SPAMCURRENTSR})
pd_add_external(spam.connect "${SPAMCURRENTSR}")
