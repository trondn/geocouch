CMAKE_MINIMUM_REQUIRED (VERSION 2.8)

PROJECT (geocouch)

LIST (APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/../tlm/cmake/Modules")
INCLUDE (FindCouchbaseErlang)

Rebar (TARGET geocouch REBAR_SCRIPT "${PROJECT_SOURCE_DIR}/rebar")
Rebar (TARGET geocouch_makecheck REBAR_SCRIPT "${PROJECT_SOURCE_DIR}/rebar"
  REBAR_OPTS -D makecheck NOALL)

ADD_CUSTOM_TARGET (geocouch_check COMMAND "${CMAKE_COMMAND}"
  -D "PROVE_EXECUTABLE=${PROVE_EXECUTABLE}"
  -D "COUCH_SRC=../cmake/couchdb/src"
  -P "${PROJECT_SOURCE_DIR}/cmake_modules/do-test.cmake"
  WORKING_DIRECTORY "${PROJECT_SOURCE_DIR}" VERBATIM)
ADD_DEPENDENCIES (geocouch_check geocouch-clean geocouch_makecheck)

INSTALL (DIRECTORY ebin DESTINATION lib/couchdb/plugins/geocouch)
INSTALL (DIRECTORY etc/couchdb/default.d DESTINATION etc/couchdb)
