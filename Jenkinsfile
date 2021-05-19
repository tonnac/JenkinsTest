@Library('UE4_Library@main')

def z = new org.foo.Zot()

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
                    z.checkOutFrom()
				}
			}
		}
	}
}