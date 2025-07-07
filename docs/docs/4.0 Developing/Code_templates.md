# Code templates - GridLAB-D Wiki

**Source URL:** https://gridlab-d.shoutwiki.com/wiki/Code_templates
# Code templates

## Contents

  * 1 Required code
    * 1.1 _module_ /main.cpp
    * 1.2 _module_ /_class_.h
    * 1.3 _module_ /_class_.cpp
    * 1.4 _module_ /Makefile.am
    * 1.5 _module_ /_module_.vcproj
  * 2 Optional code
  * 3 See also
Code templates \- Code templates for module developers 

The code templates may be downloaded to create a new module or class. In each template you will find a number of instances of <<<KEYWORD>>>, indicating that a substitution is required to properly implement your module or class. In addition, you may find a number of **TODO** comments indicating where you may have to write additional code to properly implemented the desired functionality. 

The following keywords are used: 

<<<MODULE>>>
    The name of the module
<<<YEAR>>>
    The year (or year range) of the copyright
<<<COMPANY>>>
    The name of copyright holder, i.e., the author or company to which the copyright is assigned
<<<CLASS>>>
    The class you are implementing
<<<GUID>>>
    A global unique identifier (see [GUID] for details)

## Required code

### _module_ /main.cpp
    
    
    // $ Id$
    // Copyright (c) <<<YEAR>>> <<<COMPANY>>>
    // Template valid as of [Hassayampa (Version 3.0)]
    #define DLMAIN // required to enable module main code in gridlabd.h
    #include "gridlabd.h"
    #include "<<<CLASS>>>.h"
    // **TODO** add class includes here
    // **TODO** define [module globals] here
    EXPORT class *init([CALLBACKS] *fntable, module *module, int argc, char *argv[])
    {
            if (set_callback(fntable)==NULL)
            {
                    errno = EINVAL;
                    return NULL;
            }
            // **TODO** publish [module globals] here
            new <<<CLASS>>>(module);
            // **TODO** add other classes here
            // always return the first class registered
            return <<<CLASS>>>::oclass;
    }
    EXPORT int do_kill(void*)
    {
            // **TODO** module cleanup if any
            return 0;
    }
    // **TODO** add [optional functions]
    

### _module_ /_class_.h
    
    
    // $ Id$
    // Copyright (c) <<<YEAR>>> <<<COMPANY>>>
    // Template valid as of [Hassayampa (Version 3.0)]
    #ifndef _<<<CLASS>>>_H
    #define _<<<CLASS>>>_H
    #include "gridlabd.h"
    class <<<CLASS>>> : public gld_object {
    public: // published variables
      // **TODO** add public typedefs
      // **TODO** declare published variables using [GL_* macros]
    private: // unpublished variables
      // **TODO** add private typedefs
      // **TODO** add unpublished variables
    public: // required functions
      <<<CLASS>>>(module *module);
      int create(void);
      int init(OBJECT *parent);
      // **TODO** add optional [class functions]
    public: // optional/user-defined functions
      // **TODO** add published [class functions]
    private: // internal functions
      // **TODO** add desired internal functions
    public: // required members
      static class *oclass;
      static <<<CLASS>>> *defaults;
    };
    #endif // _<<<CLASS>>>_H
    

### _module_ /_class_.cpp
    
    
    // $ Id$
    // Copyright (c) <<<YEAR>>> <<<COMPANY>>>
    // Template valid as of [Hassayampa (Version 3.0)]
    #include "<<<CLASS>>>.h"
    EXPORT_CREATE(<<<CLASS>>>);
    EXPORT_INIT(<<<CLASS>>>);
    // **TODO** add [optional functions declarations]
    class <<<CLASS>>>::oclass = NULL;
    <<<CLASS>>> *<<<CLASS>>>::defaults = NULL;
    // **TODO** add declaration of [class globals]
    <<<CLASS>>>::<<<CLASS>>>(module *module)
    {
      if ( oclass!=NULL )
        exception("cannot register class more than once");
      oclass = gld_class::create(module,"<<<CLASS>>>",sizeof(<<<CLASS>>>),<<<OPTIONS>>>);
      if ( oclass==NULL )
        exception("class registration failed");
      oclass->trl = <<<TRL>>>;
      if ( gl_publish_variable(oclass, <<<VARIABLESPECS>>>, NULL)<1 )
        exception("unable to publish properties");
      memset(defaults=this,0,sizeof(*this));
      // **TODO** set defaults
    }
    <<<CLASS>>>::create(void)
    {
      memcpy(this,defaults,sizeof(*this));
      // **TODO** set defaults
      return SUCCESS; // return FAILED on create error
    }
    <<<CLASS>>>::init(OBJECT *parent)
    {
      // **TODO** initialize object
      return SUCCESS; // return FAILED on create error
    }
    // **TODO** add implementations of [optional class functions]
    

### _module_ /Makefile.am
    
    
    pkglib_LTLIBRARIES = <<<MODULE>>>.la
    <<<MODULE>>>_la_SOURCES = main.cpp \
    	<<<CLASS>>>.cpp <<<CLASS>>>.h \
           # **TODO** add new classes before this line
    <<<MODULE>>>_la_LDFLAGS = -module -no-undefined -avoid-version -version-info 1:0:0
    
    uninstall-hook:
    	-rmdir $(DESTDIR)$(pkglibdir)
    

### _module_ /_module_.vcproj
    
    
    <?xml version="1.0" encoding="Windows-1252"?>
    <VisualStudioProject
    	ProjectType="Visual C++"
    	Version="8.00"
    	Name="<<<MODULE>>>"
    	ProjectGUID="{<<<GUID>>>}"
    	RootNamespace="gridlabd"
    	Keyword="Win32Proj"
    	>
    	<Platforms>
    		<Platform Name="Win32" />
    		<Platform Name="x64" />
    	</Platforms>
    	<ToolFiles>
    	</ToolFiles>
    	<Configurations>
    		<Configuration
    			Name="Debug|Win32"
    			OutputDirectory="..\VS2005\$(PlatformName)\$(ConfigurationName)"
    			IntermediateDirectory="$(PlatformName)\$(ConfigurationName)\$(ProjectName)"
    			ConfigurationType="2"
    			CharacterSet="2"
    			DeleteExtensionsOnClean="*.obj;*.ilk;*.tlb;*.tli;*.tlh;*.tmp;*.rsp;*.pgc;*.pgd;$(TargetPath)"
    			>
    			<Tool Name="VCPreBuildEventTool" />
    			<Tool Name="VCCustomBuildTool" />
    			<Tool Name="VCXMLDataGeneratorTool" />
    			<Tool Name="VCWebServiceProxyGeneratorTool" />
    			<Tool Name="VCMIDLTool" />
    			<Tool Name="VCCLCompilerTool"
    				Optimization="0"
    				AdditionalIncludeDirectories="..\core;"
    				PreprocessorDefinitions="WIN32;_DEBUG;_WINDOWS;_USRDLL;_CRT_SECURE_NO_DEPRECATE;_TESTING"
    				MinimalRebuild="true"
    				BasicRuntimeChecks="3"
    				RuntimeLibrary="3"
    				UsePrecompiledHeader="0"
    				WarningLevel="3"
    				Detect64BitPortabilityProblems="true"
    				DebugInformationFormat="4"
    				DisableSpecificWarnings="4996"
    			/>
    			<Tool Name="VCManagedResourceCompilerTool" />
    			<Tool Name="VCResourceCompilerTool" />
    			<Tool Name="VCPreLinkEventTool" />
    			<Tool Name="VCLinkerTool"
    				LinkIncremental="2"
    				AdditionalLibraryDirectories="$(OutDir)"
    				GenerateDebugInformation="true"
    				SubSystem="2"
    				TargetMachine="1"
    			/>
    			<Tool Name="VCALinkTool" />
    			<Tool Name="VCManifestTool" />
    			<Tool Name="VCXDCMakeTool" />
    			<Tool Name="VCBscMakeTool" />
    			<Tool Name="VCFxCopTool" />
    			<Tool Name="VCAppVerifierTool" />
    			<Tool Name="VCWebDeploymentTool" />
    			<Tool Name="VCPostBuildEventTool" />
    		</Configuration>
    		<Configuration
    			Name="Debug|x64"
    			OutputDirectory="..\VS2005\$(PlatformName)\$(ConfigurationName)"
    			IntermediateDirectory="$(PlatformName)\$(ConfigurationName)\$(ProjectName)"
    			ConfigurationType="2"
    			CharacterSet="2"
    			DeleteExtensionsOnClean="*.obj;*.ilk;*.tlb;*.tli;*.tlh;*.tmp;*.rsp;*.pgc;*.pgd;$(TargetPath)"
    			>
    			<Tool Name="VCPreBuildEventTool" />
    			<Tool Name="VCCustomBuildTool" />
    			<Tool Name="VCXMLDataGeneratorTool" />
    			<Tool Name="VCWebServiceProxyGeneratorTool" />
    			<Tool Name="VCMIDLTool"
    				TargetEnvironment="3"
    			/>
    			<Tool Name="VCCLCompilerTool"
    				Optimization="0"
    				AdditionalIncludeDirectories="..\core;"
    				PreprocessorDefinitions="WIN32;_DEBUG;_WINDOWS;_USRDLL;_CRT_SECURE_NO_DEPRECATE;_TESTING"
    				MinimalRebuild="true"
    				BasicRuntimeChecks="3"
    				RuntimeLibrary="3"
    				UsePrecompiledHeader="0"
    				WarningLevel="3"
    				Detect64BitPortabilityProblems="true"
    				DebugInformationFormat="3"
    				DisableSpecificWarnings="4996"
    			/>
    			<Tool Name="VCManagedResourceCompilerTool" />
    			<Tool Name="VCResourceCompilerTool" />
    			<Tool Name="VCPreLinkEventTool" />
    			<Tool Name="VCLinkerTool"
    				LinkIncremental="2"
    				AdditionalLibraryDirectories=""$(OutDir)";"$(SolutionDir)$(PlatformName)\$(ConfigurationName)""
    				GenerateDebugInformation="true"
    				SubSystem="2"
    				TargetMachine="17"
    			/>
    			<Tool Name="VCALinkTool" />
    			<Tool Name="VCManifestTool" />
    			<Tool Name="VCXDCMakeTool" />
    			<Tool Name="VCBscMakeTool" />
    			<Tool Name="VCFxCopTool" />
    			<Tool Name="VCAppVerifierTool" />
    			<Tool Name="VCWebDeploymentTool" />
    			<Tool Name="VCPostBuildEventTool" />
    		</Configuration>
    		<Configuration
    			Name="Release|Win32"
    			OutputDirectory="..\VS2005\$(PlatformName)\$(ConfigurationName)"
    			IntermediateDirectory="$(PlatformName)\$(ConfigurationName)\$(ProjectName)"
    			ConfigurationType="2"
    			UseOfMFC="1"
    			CharacterSet="2"
    			DeleteExtensionsOnClean="*.obj;*.ilk;*.tlb;*.tli;*.tlh;*.tmp;*.rsp;*.pgc;*.pgd;$(TargetPath)"
    			WholeProgramOptimization="1"
    			>
    			<Tool   Name="VCPreBuildEventTool" />
    			<Tool   Name="VCCustomBuildTool" />
    			<Tool   Name="VCXMLDataGeneratorTool" />
    			<Tool   Name="VCWebServiceProxyGeneratorTool" />
    			<Tool   Name="VCMIDLTool" />
    			<Tool   Name="VCCLCompilerTool"
    				AdditionalIncludeDirectories="..\core;"
    				PreprocessorDefinitions="WIN32;NDEBUG;_WINDOWS;_USRDLL;_CRT_SECURE_NO_DEPRECATE;_NO_CPPUNIT"
    				RuntimeLibrary="0"
    				UsePrecompiledHeader="0"
    				WarningLevel="3"
    				Detect64BitPortabilityProblems="true"
    				DebugInformationFormat="3"
    				DisableSpecificWarnings="4996"
    			/>
    			<Tool   Name="VCManagedResourceCompilerTool" />
    			<Tool   Name="VCResourceCompilerTool" />
    			<Tool   Name="VCPreLinkEventTool" />
    			<Tool   Name="VCLinkerTool"
    				LinkIncremental="1"
    				AdditionalLibraryDirectories="$(OutDir)"
    				GenerateDebugInformation="true"
    				SubSystem="2"
    				OptimizeReferences="2"
    				EnableCOMDATFolding="2"
    				TargetMachine="1"
    			/>
    			<Tool   Name="VCALinkTool" />
    			<Tool   Name="VCManifestTool" />
    			<Tool   Name="VCXDCMakeTool" />
    			<Tool   Name="VCBscMakeTool" />
     			<Tool   Name="VCFxCopTool" />
    			<Tool   Name="VCAppVerifierTool" />
    			<Tool   Name="VCWebDeploymentTool" />
    			<Tool   Name="VCPostBuildEventTool" />
    		</Configuration>
    		<Configuration
    			Name="Release|x64"
    			OutputDirectory="..\VS2005\$(PlatformName)\$(ConfigurationName)"
    			IntermediateDirectory="$(PlatformName)\$(ConfigurationName)\$(ProjectName)"
    			ConfigurationType="2"
    			UseOfMFC="1"
    			CharacterSet="2"
    			DeleteExtensionsOnClean="*.obj;*.ilk;*.tlb;*.tli;*.tlh;*.tmp;*.rsp;*.pgc;*.pgd;$(TargetPath)"
    			WholeProgramOptimization="1"
    			>
    			<Tool   Name="VCPreBuildEventTool" />
    			<Tool   Name="VCCustomBuildTool" />
    			<Tool   Name="VCXMLDataGeneratorTool" />
    			<Tool   Name="VCWebServiceProxyGeneratorTool" />
    			<Tool   Name="VCMIDLTool"
    				TargetEnvironment="3"
    			/>
    			<Tool   Name="VCCLCompilerTool"
    				AdditionalIncludeDirectories="..\core;"
    				PreprocessorDefinitions="WIN32;NDEBUG;_WINDOWS;_USRDLL;_CRT_SECURE_NO_DEPRECATE;_NO_CPPUNIT"
    				RuntimeLibrary="0"
    				UsePrecompiledHeader="0"
    				WarningLevel="3"
    				Detect64BitPortabilityProblems="true"
    				DebugInformationFormat="3"
    				DisableSpecificWarnings="4996"
    			/>
    			<Tool   Name="VCManagedResourceCompilerTool" />
    			<Tool   Name="VCResourceCompilerTool" />
    			<Tool   Name="VCPreLinkEventTool" />
    			<Tool   Name="VCLinkerTool"
    				LinkIncremental="1"
    				AdditionalLibraryDirectories=""$(OutDir)";"$(SolutionDir)$(PlatformName)\$(ConfigurationName)""
    				GenerateDebugInformation="true"
    				SubSystem="2"
    				OptimizeReferences="2"
    				EnableCOMDATFolding="2"
    				TargetMachine="17"
    			/>
    			<Tool   Name="VCALinkTool" />
    			<Tool   Name="VCManifestTool" />
    			<Tool   Name="VCXDCMakeTool"  />
    			<Tool   Name="VCBscMakeTool" />
    			<Tool   Name="VCFxCopTool" />
    			<Tool  Name="VCAppVerifierTool" />
    			<Tool  Name="VCWebDeploymentTool" />
    			<Tool   Name="VCPostBuildEventTool" />
     		</Configuration>
    	</Configurations>
    	
    	<Files>
    		<Filter
    			Name="Source Files"
    			Filter="cpp;c;cc;cxx;def;odl;idl;hpj;bat;asm;asmx"
    			UniqueIdentifier="{<<<GUID>>>}"
    			>
    			<File RelativePath=".\<<<CLASS>>>.cpp" >
    			</File>
    			<File RelativePath=".\main.cpp" >
    			</File>
    		</Filter>
    		<Filter
    			Name="Header Files"
    			Filter="h;hpp;hxx;hm;inl;inc;xsd"
    			UniqueIdentifier="{<<<GUID>>>}"
    			>
    			<File RelativePath=".\<<<CLASS>>>.h" >
    			</File>
    		</Filter>
    		<Filter
    			Name="Test files"
    			>
    			<File RelativePath=".\autotest\test_<<<CLASS>>>.glm" >
    			</File>
    		</Filter>
    		<File
    			RelativePath=".\Makefile.am"
     			>
    		</File>
    	</Files>
    	<Globals>
    	</Globals>
    </VisualStudioProject>
    

## Optional code

**TODO**: 

## See also

  * [Guide to Programming GridLAB-D]
    * Introduction 
      * [Developer prerequisites]
      * [Programming conventions]
      * [Build/release process]
      * [Documentation Guide]
      * [Theory of operation]
    * [Creating a module]
      * [Module globals]
      * [Module functions]
      * [Subsecond processing]
      * [Import/export]
      * [Check]
      * [KML output]
      * [Example 1]
    * [Creating a class]
      * [Class functions]
      * [Class globals]
      * [Publishing properties]
      * [Publishing methods]
      * [Notifications]
      * [Load methods] 
      * [Example 2]
    * Special Topics 
      * [Data types]
      * [Multithreading]
      * [Application links]
      * [Realtime server]
      * [Graphical user interfaces]
      * [Troubleshooting messages]
      * [Example 3]
    * [Source documentation]
      * [C/C++ Module API documentation (trunk)](http://gridlab-d.sourceforge.net/doxygen/trunk/group__module__api.html)
      * [C/C++ Module API Guide]
      * [Example 4]
    * [Validation]
      * [Example 5]
    * Debugging 
      * [Debug option]
      * [VS2005 (MS Windows)]
        * [use_msvc]
      * [gdb option (linux/mac)]
        * [gdb_window]
      * [Runtime Class Debugging]
        * [compile_once]
    * Code templates
