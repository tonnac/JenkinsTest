@Library('UE4_Library@main')

// def z = new org.foo.Zot()

// pipeline 
// {
// 	agent any
	
// 	stages
// 	{
// 		stage('Generate Project Files')
// 		{
// 			steps
// 			{
// 				script
// 				{
//                     z.checkOutFrom()
// 				}
// 			}
// 		}
// 	}
// }


def UE4 = new unreal.UE4()

def BuildConfigChoices = UE4.GetBuildConfigurationChoices()

pipeline 
{
	agent any
    
	options 
	{ skipDefaultCheckout() }
    
	parameters
	{
		choice(
			choices: BuildConfigChoices,
			description: "Build Configuration",
			name: "BuildConfig"
			)
		booleanParam(defaultValue: true, description: 'Should the project be cooked?', name: 'CookProject')
		string(defaultValue: 'ThirdPersonMap', description: 'Maps we want to cook', name: 'MapsToCook')
	}
    
	environment 
	{
		ProjectName		= getFolderName(this)
		WorkspaceRootDir	= "${env.WORKSPACE}"
		
		UE4 = UE4.Initialise(ProjectName, ProjectRootDir)
	}
	
	stages
	{
		stage('Generate Project Files')
		{
			steps
			{
				script
				{
                    echo "NODE_NAME = ${env.WorkspaceRootDir}"
                    echo "NODE_NAME = ${env.ProjectName}"
                    echo "NODE_NAME = ${env.NODE_NAME}"
                    echo "NODE_NAME = ${env.ENGINE_ROOT}"
                    echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL} ${env.WORKSPACE}"
                    echo sh(script: 'env|sort', returnStdout: true)
					UE4.GenerateProjectFiles()
				}
			}
		}
		stage('Compile')
		{
			steps
			{
				script
				{
					UE4.CompileProject(params.BuildConfig as unreal.BuildConfiguration)
				}
			}
		}
		stage('Cook')
		{
			when
			{
				expression { params.CookProject == true }
			}
			steps
			{
				script
				{
					UE4.CookProject("WindowsNoEditor", "${params.MapsToCook}")
				}
			}
		}
		stage('Build DDC') 
		{
			steps
			{
				script
				{
					UE4.BuildDDC()
				}
			}
		}
	}
}