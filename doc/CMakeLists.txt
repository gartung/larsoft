
#
# Add a target to generate API documentation with Doxygen.
# 
# The procedure to create and install the documentation is in multiple steps.
# 
# ***                                                               ***
# *** By default, no documentation will be automatically generated! ***
# ***                                                               ***
# 
# To generate the documentation, the target 'doc' should be invoked:
#     
#     cd "$MRB_BUILDDIR"
#     make doc              # or `ninja doc`
#     
# Afterwards, when installation is requested, the documentation will be
# installed in "${MRB_INSTALL}/doc". This means that the documentation is not
# in any UPS package (this can be changed below).
# 
# 
find_package(Doxygen)
if(DOXYGEN_FOUND)
  
  # prepare the Doxygen configuration from our template;
  # this will replace "@VARNAME@" tags with the value of cmake variable VARNAME;
  # the @ONLY option stops cmake from doingthe same with "${VARNAME}" tags.
  configure_file(SourceDoxyfile.in SourceDoxyfile @ONLY)
  configure_file(LocalSourceDoxyfile.in LocalSourceDoxyfile @ONLY)
  configure_file(FullSourceDoxyfile.in FullSourceDoxyfile @ONLY)
  
  # copy the stuff we need to give to Doxygen
  file(COPY LArSoftLogo.png
    DESTINATION .
    )
  
  # create a new target "doc" to compile the documentation of the code in the
  # source directory
  add_custom_target(doc
    ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/LocalSourceDoxyfile
    COMMENT "Generating LArSoft documentation from source area"
    USES_TERMINAL
    VERBATIM
    )
  
  # create a new target "fulldoc" to compile the documentation
  add_custom_target(fulldoc
    ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/FullSourceDoxyfile
    COMMENT "Generating complete LArSoft documentation including source area"
    USES_TERMINAL
    VERBATIM
    )
  
  # install the documentation of the whole suite into larsoft package;
  # it is optional, so if the doc targets have not run, this won't fail;
  # note that the destination is the 'doc' directory under $MRB_INSTALL;
  # another reasonable value would be "larsoft/doc", to include it in the UPS
  # product "larsoft"
  install(
    DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/html
    DESTINATION doc
    OPTIONAL
    )
  
else(DOXYGEN_FOUND)
  message(WARNING "Doxygen not found. The target \"doc\" won't be available.")
endif(DOXYGEN_FOUND)
