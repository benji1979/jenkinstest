pipeline {
    
	
	agent {
        label 'Windows-build' 
    }	

    stages {
        stage('Checkout') {
            steps {
                // A Jenkins automatikusan letölti a kódot a GitHub-ról a munkakönyvtárba,
                // és a 'clean: true' opcióval törli a korábbi felesleges fájlokat (mint az 'rd /s /q')
                checkout scm: [
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    extensions: [[$class: 'CleanCheckout']], 
                    userRemoteConfigs: scm.userRemoteConfigs
                ]
            }
        }

        stage('NuGet Restore') {
            steps {
                // Belépünk a TestApplication mappába és futtatjuk a célzott restore-t
                dir('TestApplication') {
                    bat 'dotnet restore TestApplication.sln -r win-x86'
                }
            }
        }

        stage('Publish') {
            steps {
                dir('TestApplication') {
                    // Lefuttatjuk a Publish-t a FolderProfile2 profil alapján.
                    // A logfájlba irányítást (>build.log) elhagyhatod, mert a Jenkins 
                    // a teljes konzolos kimenetet automatikusan elmenti és mutatja a webes felületen.
                    bat '"C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\msbuild" TestApplication.sln /t:Publish /p:Configuration=Release /p:PublishProfile=FolderProfile2'
                }
            }
        }
    }
}
