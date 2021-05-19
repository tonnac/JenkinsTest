@Library('UE4_Library')

def z = new org.foo.Zot()
z.checkOutFrom(repo)

pipeline 
{
	agent any
	
	stages
	{
		stage('Generate Project Files')
		{
			steps
			{
				script
				{
                    z.checkOutFrom(repo)
				}
			}
		}
	}
}