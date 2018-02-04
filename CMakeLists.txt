cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)
set(CMAKE_PREFIX_PATH C:/Qt/Qt5.6.3/5.6.3/msvc2013_64)

include_directories(${CMAKE_BINARY_DIR}
    src
    trapcoin/external
    trapcoin/include
    trapcoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("trapcoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
   trapcoin/external/miniupnpc/connecthostport.c
  trapcoin/external/miniupnpc/igd_desc_parse.c
  trapcoin/external/miniupnpc/minisoap.c
  trapcoin/external/miniupnpc/miniupnpc.c
  trapcoin/external/miniupnpc/miniwget.c
  trapcoin/external/miniupnpc/minixml.c
  trapcoin/external/miniupnpc/portlistingparse.c
  trapcoin/external/miniupnpc/receivedata.c
  trapcoin/external/miniupnpc/upnpcommands.c
  trapcoin/external/miniupnpc/upnpreplyparse.c
  trapcoin/src/Common/Base58.cpp
  trapcoin/src/Common/CommandLine.cpp
  trapcoin/src/Common/Util.cpp
  trapcoin/src/Common/StringTools.cpp
  trapcoin/src/Common/JsonValue.cpp
  trapcoin/src/Common/ConsoleTools.cpp
  trapcoin/src/Common/MemoryInputStream.cpp
  trapcoin/src/Common/PathTools.cpp
  trapcoin/src/Common/StdInputStream.cpp
  trapcoin/src/Common/StdOutputStream.cpp
  trapcoin/src/Common/StreamTools.cpp
  trapcoin/src/Common/StringOutputStream.cpp
  trapcoin/src/Common/StringView.cpp
  trapcoin/src/Common/VectorOutputStream.cpp
  trapcoin/src/crypto/blake256.c
  trapcoin/src/crypto/chacha8.c
  trapcoin/src/crypto/crypto-ops-data.c
  trapcoin/src/crypto/crypto-ops.c
  trapcoin/src/crypto/crypto.cpp
  trapcoin/src/crypto/groestl.c
  trapcoin/src/crypto/hash-extra-blake.c
  trapcoin/src/crypto/hash-extra-groestl.c
  trapcoin/src/crypto/hash-extra-jh.c
  trapcoin/src/crypto/hash-extra-skein.c
  trapcoin/src/crypto/hash.c
  trapcoin/src/crypto/jh.c
  trapcoin/src/crypto/keccak.c
  trapcoin/src/crypto/oaes_lib.c
  trapcoin/src/crypto/random.c
  trapcoin/src/crypto/skein.c
  trapcoin/src/crypto/slow-hash.c
  trapcoin/src/crypto/slow-hash.cpp
  trapcoin/src/crypto/tree-hash.c
  trapcoin/src/CryptoNoteCore/BlockchainIndices.cpp
  trapcoin/src/CryptoNoteCore/BlockchainMessages.cpp
  trapcoin/src/CryptoNoteCore/BlockIndex.cpp
  trapcoin/src/CryptoNoteCore/CoreConfig.cpp
  trapcoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  trapcoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  trapcoin/src/CryptoNoteCore/Currency.cpp
  trapcoin/src/CryptoNoteCore/MinerConfig.cpp
  trapcoin/src/CryptoNoteCore/Transaction.cpp
  trapcoin/src/CryptoNoteCore/Account.cpp
  trapcoin/src/CryptoNoteCore/Blockchain.cpp
  trapcoin/src/CryptoNoteCore/Checkpoints.cpp
  trapcoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  trapcoin/src/CryptoNoteCore/Core.cpp
  trapcoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  trapcoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  trapcoin/src/CryptoNoteCore/Difficulty.cpp
  trapcoin/src/CryptoNoteCore/IBlock.cpp
  trapcoin/src/CryptoNoteCore/Miner.cpp
  trapcoin/src/CryptoNoteCore/TransactionExtra.cpp
  trapcoin/src/CryptoNoteCore/TransactionPool.cpp
  trapcoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  trapcoin/src/CryptoNoteCore/TransactionUtils.cpp
  trapcoin/src/InProcessNode/InProcessNode.cpp
  trapcoin/src/InProcessNode/InProcessNodeErrors.cpp
  trapcoin/src/NodeRpcProxy/NodeErrors.cpp
  trapcoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  trapcoin/src/P2p/NetNodeConfig.cpp
  trapcoin/src/Serialization/BinaryInputStreamSerializer.cpp
  trapcoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  trapcoin/src/Serialization/JsonInputValueSerializer.cpp
  trapcoin/src/Serialization/JsonOutputStreamSerializer.cpp
  trapcoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  trapcoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  trapcoin/src/Serialization/SerializationOverloads.cpp
  trapcoin/src/Transfers/BlockchainSynchronizer.cpp
  trapcoin/src/Transfers/SynchronizationState.cpp
  trapcoin/src/Transfers/TransfersConsumer.cpp
  trapcoin/src/Transfers/TransfersContainer.cpp
  trapcoin/src/Transfers/TransfersSubscription.cpp
  trapcoin/src/Transfers/TransfersSynchronizer.cpp
  trapcoin/src/Wallet/LegacyKeysImporter.cpp
  trapcoin/src/Wallet/WalletAsyncContextCounter.cpp
  trapcoin/src/Wallet/WalletErrors.cpp
  trapcoin/src/WalletLegacy/KeysStorage.cpp
  trapcoin/src/WalletLegacy/WalletLegacy.cpp
  trapcoin/src/WalletLegacy/WalletHelper.cpp
  trapcoin/src/WalletLegacy/WalletLegacySerializer.cpp
  trapcoin/src/WalletLegacy/WalletLegacySerialization.cpp
  trapcoin/src/WalletLegacy/WalletTransactionSender.cpp
  trapcoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  trapcoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  trapcoin/src/System/ContextGroup.cpp
  trapcoin/src/System/Event.cpp
  trapcoin/src/System/EventLock.cpp
  trapcoin/src/System/InterruptedException.cpp
  trapcoin/src/System/Ipv4Address.cpp
  trapcoin/src/System/TcpStream.cpp
  trapcoin/src/HTTP/HttpRequest.cpp
  trapcoin/src/HTTP/HttpParser.cpp
  trapcoin/src/HTTP/HttpParserErrorCodes.cpp
  trapcoin/src/HTTP/HttpResponse.cpp
  trapcoin/src/Rpc/HttpClient.cpp
  trapcoin/src/Rpc/JsonRpc.cpp
  trapcoin/src/P2p/NetNode.cpp
  trapcoin/src/P2p/LevinProtocol.cpp
  trapcoin/src/P2p/NetNodeConfig.cpp
  trapcoin/src/P2p/PeerListManager.cpp
  trapcoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  trapcoin/src/Logging/ILogger.cpp
  trapcoin/src/Logging/LoggerGroup.cpp
  trapcoin/src/Logging/CommonLogger.cpp
  trapcoin/src/Logging/LoggerManager.cpp
  trapcoin/src/Logging/FileLogger.cpp
  trapcoin/src/Logging/StreamLogger.cpp
  trapcoin/src/Logging/ConsoleLogger.cpp
  trapcoin/src/Logging/LoggerMessage.cpp
  trapcoin/src/Logging/LoggerRef.cpp
  trapcoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  trapcoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  trapcoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(trapcoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} trapcoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(trapcoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES trapcoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
