node (label: 'FortifySCA')
{  
    stage('Git')
    {
        git 'https://github.com/bm-sushma/musicsite.git'
    }
    stage('Code Build')
    {
        powershell label: '', script: '''Remove-Item c:\\webpackage -Recurse -ErrorAction Ignore

        C:\\MSBUILD\\MSBuild\\15.0\\Bin\\MSBuild.exe "C:\\Users\\Vivek001\\jenkins\\workspace\\MusicSite-DotNet\\MusicSite\\MusicSite.csproj" /p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:PackageLocation="c:\\webpackage" /p:DeployIisAppPath="Default Web Site"

        Copy-Item  -Path "C:\\webpackage\\MusicSite.zip" -Destination "C:\\Users\\Vivek001\\jenkins\\workspace\\MusicSite-DotNet" -Recurse -force'''
    }
    stage('Fortify Clean')
    {
        fortifyClean addJVMOptions: '', buildID: 'MusicSite-DotNet', logFile: '', maxHeap: ''
    }   
    stage('Fortify Update')
    {
        fortifyUpdate proxyPassword: '', proxyURL: '', proxyUsername: '', updateServerURL: 'https://update.fortify.com'
    }
    stage('Fortify Translate')
    {
       fortifyTranslate addJVMOptions: '', buildID: 'MusicSite-DotNet', excludeList: '', logFile: '', maxHeap: '', projectScanType: fortifyAdvanced(advOptions: '"C:\\Program Files (x86)\\MSBuild\\14.0\\Bin\\msbuild.exe" MusicSite\\MusicSite.csproj')
    }
    stage('Fortify Scan')
    {
        fortifyScan addJVMOptions: '', addOptions: '', buildID: 'MusicSite-DotNet', customRulepacks: '', logFile: '', maxHeap: '', resultsFile: 'MusicSite.fpr'
    }
    stage('Fortify Upload')
    {
        fortifyUpload appName: 'Music', appVersion: '1.1', failureCriteria: '', filterSet: '', pollingInterval: '1', resultsFile: 'MusicSite.fpr'
    }
  
} 
