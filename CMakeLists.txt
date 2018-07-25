cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB discoverycoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    discoverycoin/external
    discoverycoin/include
    discoverycoin/src)

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
configure_file("discoverycoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  discoverycoin/external/miniupnpc/connecthostport.c
  discoverycoin/external/miniupnpc/igd_desc_parse.c
  discoverycoin/external/miniupnpc/minisoap.c
  discoverycoin/external/miniupnpc/miniupnpc.c
  discoverycoin/external/miniupnpc/miniwget.c
  discoverycoin/external/miniupnpc/minixml.c
  discoverycoin/external/miniupnpc/portlistingparse.c
  discoverycoin/external/miniupnpc/receivedata.c
  discoverycoin/external/miniupnpc/upnpcommands.c
  discoverycoin/external/miniupnpc/upnpreplyparse.c
  discoverycoin/src/Common/Base58.cpp
  discoverycoin/src/Common/CommandLine.cpp
  discoverycoin/src/Common/Util.cpp
  discoverycoin/src/Common/StringTools.cpp
  discoverycoin/src/Common/JsonValue.cpp
  discoverycoin/src/Common/ConsoleTools.cpp
  discoverycoin/src/Common/MemoryInputStream.cpp
  discoverycoin/src/Common/PathTools.cpp
  discoverycoin/src/Common/StdInputStream.cpp
  discoverycoin/src/Common/StdOutputStream.cpp
  discoverycoin/src/Common/StreamTools.cpp
  discoverycoin/src/Common/StringOutputStream.cpp
  discoverycoin/src/Common/StringView.cpp
  discoverycoin/src/Common/VectorOutputStream.cpp
  discoverycoin/src/crypto/blake256.c
  discoverycoin/src/crypto/chacha8.c
  discoverycoin/src/crypto/crypto-ops-data.c
  discoverycoin/src/crypto/crypto-ops.c
  discoverycoin/src/crypto/crypto.cpp
  discoverycoin/src/crypto/groestl.c
  discoverycoin/src/crypto/hash-extra-blake.c
  discoverycoin/src/crypto/hash-extra-groestl.c
  discoverycoin/src/crypto/hash-extra-jh.c
  discoverycoin/src/crypto/hash-extra-skein.c
  discoverycoin/src/crypto/hash.c
  discoverycoin/src/crypto/jh.c
  discoverycoin/src/crypto/keccak.c
  discoverycoin/src/crypto/oaes_lib.c
  discoverycoin/src/crypto/random.c
  discoverycoin/src/crypto/skein.c
  discoverycoin/src/crypto/slow-hash.c
  discoverycoin/src/crypto/slow-hash.cpp
  discoverycoin/src/crypto/tree-hash.c
  discoverycoin/src/CryptoNoteCore/BlockchainIndices.cpp
  discoverycoin/src/CryptoNoteCore/BlockchainMessages.cpp
  discoverycoin/src/CryptoNoteCore/BlockIndex.cpp
  discoverycoin/src/CryptoNoteCore/CoreConfig.cpp
  discoverycoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  discoverycoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  discoverycoin/src/CryptoNoteCore/Currency.cpp
  discoverycoin/src/CryptoNoteCore/MinerConfig.cpp
  discoverycoin/src/CryptoNoteCore/Transaction.cpp
  discoverycoin/src/CryptoNoteCore/Account.cpp
  discoverycoin/src/CryptoNoteCore/Blockchain.cpp
  discoverycoin/src/CryptoNoteCore/Checkpoints.cpp
  discoverycoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  discoverycoin/src/CryptoNoteCore/Core.cpp
  discoverycoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  discoverycoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  discoverycoin/src/CryptoNoteCore/Difficulty.cpp
  discoverycoin/src/CryptoNoteCore/IBlock.cpp
  discoverycoin/src/CryptoNoteCore/Miner.cpp
  discoverycoin/src/CryptoNoteCore/TransactionExtra.cpp
  discoverycoin/src/CryptoNoteCore/TransactionPool.cpp
  discoverycoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  discoverycoin/src/CryptoNoteCore/TransactionUtils.cpp
  discoverycoin/src/InProcessNode/InProcessNode.cpp
  discoverycoin/src/InProcessNode/InProcessNodeErrors.cpp
  discoverycoin/src/NodeRpcProxy/NodeErrors.cpp
  discoverycoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  discoverycoin/src/P2p/NetNodeConfig.cpp
  discoverycoin/src/Serialization/BinaryInputStreamSerializer.cpp
  discoverycoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  discoverycoin/src/Serialization/JsonInputValueSerializer.cpp
  discoverycoin/src/Serialization/JsonOutputStreamSerializer.cpp
  discoverycoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  discoverycoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  discoverycoin/src/Serialization/SerializationOverloads.cpp
  discoverycoin/src/Transfers/BlockchainSynchronizer.cpp
  discoverycoin/src/Transfers/SynchronizationState.cpp
  discoverycoin/src/Transfers/TransfersConsumer.cpp
  discoverycoin/src/Transfers/TransfersContainer.cpp
  discoverycoin/src/Transfers/TransfersSubscription.cpp
  discoverycoin/src/Transfers/TransfersSynchronizer.cpp
  discoverycoin/src/Wallet/LegacyKeysImporter.cpp
  discoverycoin/src/Wallet/WalletAsyncContextCounter.cpp
  discoverycoin/src/Wallet/WalletErrors.cpp
  discoverycoin/src/WalletLegacy/KeysStorage.cpp
  discoverycoin/src/WalletLegacy/WalletLegacy.cpp
  discoverycoin/src/WalletLegacy/WalletHelper.cpp
  discoverycoin/src/WalletLegacy/WalletLegacySerializer.cpp
  discoverycoin/src/WalletLegacy/WalletLegacySerialization.cpp
  discoverycoin/src/WalletLegacy/WalletTransactionSender.cpp
  discoverycoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  discoverycoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  discoverycoin/src/System/ContextGroup.cpp
  discoverycoin/src/System/Event.cpp
  discoverycoin/src/System/EventLock.cpp
  discoverycoin/src/System/InterruptedException.cpp
  discoverycoin/src/System/Ipv4Address.cpp
  discoverycoin/src/System/TcpStream.cpp
  discoverycoin/src/HTTP/HttpRequest.cpp
  discoverycoin/src/HTTP/HttpParser.cpp
  discoverycoin/src/HTTP/HttpParserErrorCodes.cpp
  discoverycoin/src/HTTP/HttpResponse.cpp
  discoverycoin/src/Rpc/HttpClient.cpp
  discoverycoin/src/Rpc/JsonRpc.cpp
  discoverycoin/src/P2p/NetNode.cpp
  discoverycoin/src/P2p/LevinProtocol.cpp
  discoverycoin/src/P2p/NetNodeConfig.cpp
  discoverycoin/src/P2p/PeerListManager.cpp
  discoverycoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  discoverycoin/src/Logging/ILogger.cpp
  discoverycoin/src/Logging/LoggerGroup.cpp
  discoverycoin/src/Logging/CommonLogger.cpp
  discoverycoin/src/Logging/LoggerManager.cpp
  discoverycoin/src/Logging/FileLogger.cpp
  discoverycoin/src/Logging/StreamLogger.cpp
  discoverycoin/src/Logging/ConsoleLogger.cpp
  discoverycoin/src/Logging/LoggerMessage.cpp
  discoverycoin/src/Logging/LoggerRef.cpp
  discoverycoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  discoverycoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  discoverycoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(discoverycoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} discoverycoin/external/miniupnpc/minissdpc.c)
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

include_directories(discoverycoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES discoverycoin/src/Platform/${PLATFORM_DIR}/System/*)
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
