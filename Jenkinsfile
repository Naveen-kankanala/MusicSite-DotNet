node (label: 'FortifySCA')
{  

    def ApplicationName 	=	"Music"
    def ApplicationVersion	=  	"1.1"
    def bID		        	=	"MusicSite-DotNet"
    def GitRepoURL	       	=	"https://github.com/bm-sushma/musicsite.git"
    def FailureCriteria		=	"category: Path Manipulation"
    def ResultsFile	    	=	"MusicSite.fpr"
    def msbuild	        	=	"C:\\MSBUILD\\MSBuild\\15.0\\Bin\\MSBuild.exe"
    def project	        	=	"C:\\Users\\Vivek001\\jenkins\\workspace\\MusicSite-DotNet\\MusicSite\\MusicSite.csproj"
    def destination	    	=	"C:\\Users\\Vivek001\\jenkins\\workspace\\MusicSite-DotNet"
    def ProjectPath		=	" "
   
   
    stage('Git')
    {
        git GitRepoURL
    }
    stage('Code Build')
    {
        powershell label: '', script: '''Remove-Item c:\\webpackage -Recurse -ErrorAction Ignore

        C:\\MSBUILD\\MSBuild\\15.0\\Bin\\MSBuild.exe "C:\\Users\\Vivek001\\jenkins\\workspace\\MusicSite-DotNet\\MusicSite\\MusicSite.csproj" /p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:PackageLocation="c:\\webpackage" /p:DeployIisAppPath="Default Web Site"

        Copy-Item  -Path "C:\\webpackage\\MusicSite.zip" -Destination "C:\\Users\\Vivek001\\jenkins\\workspace\\MusicSite-DotNet" -Recurse -force'''
    }
    stage('Fortify Clean')
    {
        fortifyClean addJVMOptions: '', buildID: bID, logFile: '', maxHeap: ''
    }   
    stage('Fortify Update')
    {
        fortifyUpdate proxyPassword: '', proxyURL: '', proxyUsername: '', updateServerURL: 'https://update.fortify.com'
    }
    stage('Fortify Translate')
    {
       fortifyTranslate addJVMOptions: '', buildID: bID, excludeList: '', logFile: '', maxHeap: '', projectScanType: fortifyAdvanced(advOptions: '"C:\\Program Files (x86)\\MSBuild\\14.0\\Bin\\msbuild.exe" MusicSite\\MusicSite.csproj')
    }
    stage('Fortify Scan')
    {
        fortifyScan addJVMOptions: '', addOptions: '', buildID: bID, customRulepacks: '', logFile: '', maxHeap: '', resultsFile: ResultsFile
    }
    stage('Fortify Upload')
    {
        fortifyUpload appName: ApplicationName, appVersion: ApplicationVersion, failureCriteria: FailureCriteria, filterSet: '', pollingInterval: '1', resultsFile: ResultsFile
    }
  
} 
