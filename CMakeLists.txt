cmake_minimum_required(VERSION 3.8)
include(ExternalProject)

set(EXT_OUTPUT ${CMAKE_CURRENT_SOURCE_DIR} CACHE PATH "dependencies output")

ExternalProject_Add(amqpcpp
GIT_REPOSITORY      https://github.com/CopernicaMarketingSoftware/AMQP-CPP.git
GIT_TAG             v4.1.4
GIT_SHALLOW         true
CMAKE_ARGS          -DCMAKE_INSTALL_PREFIX=${EXT_OUTPUT})
                                       
                    
ExternalProject_Add(flann
GIT_REPOSITORY      https://github.com/mariusmuja/flann.git
GIT_TAG             1.9.1
GIT_SHALLOW         true
CMAKE_ARGS          -DCMAKE_INSTALL_PREFIX=${EXT_OUTPUT}
                    -DBUILD_DOC=OFF 
                    -DBUILD_C_BINDINGS=OFF
                    -DBUILD_MATLAB_BINDINGS=OFF 
                    -DBUILD_PYTHON_BINDINGS=OFF 
                    -DBUILD_TESTS=OFF
                    -DBUILD_EXAMPLES=OFF)
                    
                    
ExternalProject_Add(mongoc
GIT_REPOSITORY      https://github.com/mongodb/mongo-c-driver.git
GIT_TAG             1.13.0
GIT_SHALLOW         true
CMAKE_ARGS          -DCMAKE_INSTALL_PREFIX=${EXT_OUTPUT}
                    -DENABLE_TESTS=OFF
                    -DBUILD_TESTING=OFF
                    -DENABLE_SSL=AUTO
                    -DENABLE_SASL=OFF
                    -DENABLE_EXAMPLES=OFF
                    -DCMAKE_C_FLAGS="-D__USE_MINGW_ANSI_STDIO=1")
  
  
ExternalProject_Add(mongocxx
GIT_REPOSITORY      https://github.com/mongodb/mongo-cxx-driver.git
GIT_TAG             r3.4.0
GIT_SHALLOW         true
DEPENDS             mongoc
CMAKE_ARGS          -DCMAKE_INSTALL_PREFIX=${EXT_OUTPUT}
                    -DBUILD_TESTING=OFF
                    -DBSONCXX_POLY_USE_BOOST=1)
                     

ExternalProject_Add(nlohmann_json
GIT_REPOSITORY      https://github.com/nlohmann/json.git
GIT_TAG             v3.6.1
GIT_SHALLOW         true
CMAKE_ARGS          -DCMAKE_INSTALL_PREFIX=${EXT_OUTPUT}
                    -DJSON_MultipleHeaders=ON
                    -DBUILD_TESTING=OFF)
                     

ExternalProject_Add(clipp
GIT_REPOSITORY      https://github.com/muellan/clipp.git
GIT_TAG             v1.2.3
GIT_SHALLOW         true
CONFIGURE_COMMAND   ""
BUILD_COMMAND       ""
INSTALL_COMMAND     ${CMAKE_COMMAND} -E copy_directory <SOURCE_DIR>/include ${EXT_OUTPUT}/include)


ExternalProject_Add(cotire
GIT_REPOSITORY      https://github.com/sakra/cotire.git
GIT_TAG             cotire-1.8.0
GIT_SHALLOW         true
CONFIGURE_COMMAND   ""
BUILD_COMMAND       ""
INSTALL_COMMAND     ${CMAKE_COMMAND} -E copy_directory <SOURCE_DIR>/CMake ${EXT_OUTPUT}/cmake)


ExternalProject_Add(nameof
GIT_REPOSITORY      https://github.com/bravikov/nameof.git
GIT_TAG             v1.0.0
GIT_SHALLOW         true
CONFIGURE_COMMAND   ""
BUILD_COMMAND       ""
INSTALL_COMMAND     ${CMAKE_COMMAND} -E copy <SOURCE_DIR>/nameof.h ${EXT_OUTPUT}/include)   
                    

find_program(MAKE_CMD NAMES mingw32-make make gmake nmake)

ExternalProject_Add(cryptopp
GIT_REPOSITORY      https://github.com/weidai11/cryptopp.git
GIT_TAG             CRYPTOPP_7_0_0
GIT_SHALLOW         true
EXCLUDE_FROM_ALL    true
CONFIGURE_COMMAND   ""
BUILD_IN_SOURCE     true
BUILD_COMMAND       ${MAKE_CMD}
INSTALL_COMMAND     ${MAKE_CMD} install PREFIX=${EXT_OUTPUT})
