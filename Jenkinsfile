      
node {
    def address = "devops.splonline.com.sa/SPLDigitalCollection/Integration_WSO2/_git/SPLCICDTest"


    def  notify_users = ""
    def newCommitComments= '';
    def project_name = "";
    def commit_id_exist = false;
    def url = ""
    def ei_service_url=""
    def earth_service_check_url=""
    def earth_service_info_url=""
    def ei_service_related_info_html=""
    def isRunByAdmin = env.Base_EI_Project?true:false
    def build_email_title_suffix = ""    
    def props = "";
    def  Base_EI_Project = "";



try {
	  
    stage("Clear workspace"){ 
        powershell 'rm -r -fo ./*'
    }
	
    def ei_service_project_tem_path='ei_service_project_tem_path';
    def base_ei_project_tem_path='base_ei_project_tem_path';

	def commitComments = '';
    def gitURL = '';
    def gitProjectName = '';
    def commit_id = '';
	def is_car_build_error = false;
    def is_car_deploy_error = false;
	
    stage("Checkout code: current project"){
        bat "mkdir ${ei_service_project_tem_path}"
        dir("${ei_service_project_tem_path}"){
            if(env.SPLDemo){
                EI_Service_Project = env.EI_Service_Project.replaceAll("https?://","")
               git credentialsId: 'devops_svc', url: "http://${EI_Service_Project}", branch: "${env.BRANCH_NAME}"
            }else{
               checkout scm 
            }
            gitURL = bat(returnStdout: true, script: 'git remote -v| find "http*.git" /v| find "push"')
            gitProjectName  = powershell (returnStdout: true, script: "Set-Alias -Name sed -Value C:/'Program Files'/Git/usr/bin/sed.exe;git remote -v|findstr /R 'push'|findstr 'http.*git'|sed 's%^.*devopsadmin.com/%%g'|sed 's/.git\$//g'")
            build_email_title_suffix = "[(${env.BRANCH_NAME})${gitProjectName}] "
            echo "build_email_title_suffix is $build_email_title_suffix"
            commit_id = bat(returnStdout: true, script: 'git rev-parse HEAD')
            commitComments = bat(returnStdout: true, script: 'git log -1 ')
			newCommitComments= "Add new Car to ${env.BRANCH_NAME} ${gitProjectName}\nBranch:${env.BRANCH_NAME}\nCommit Comments:\n${commitComments}";
            echo "newCommitComments ${newCommitComments}"
        }
    }
 stage("Checkout code: configuration"){
    echo "Enter Checkout code: configuration"
      props = readYaml file: "${ei_service_project_tem_path}/SPLCICD/SPLDemo/jenkinsfile.pipeline.yml"
     echo "Base_EI_Project = ${env.Base_EI_Project}"
       Base_EI_Project = getNotNullValue(env.Base_EI_Project,props.base_ei_project)
	 echo "Base_EI_Project = ${Base_EI_Project}"
	      Base_EI_Project = getNotNullValue(Base_EI_Project,address);
          Base_EI_Project = Base_EI_Project.replaceAll("https?://","")
	 echo "Base_EI_Project = ${Base_EI_Project}"
     def  is_vm_target_system = "${Base_EI_Project}".contains(".git"); 
	 def is_need_build = env.BRANCH_NAME != 'main' || isRunByAdmin ;
	 notify_users = concatNotifyUsers(props.notify_users,env.Notify_Users);
    echo "Exit Checkout code: configuration"
 }
  echo "Base_EI_Project = ${Base_EI_Project}"
	//  if(!is_need_build){
	 
	//     //  currentBuild.setDescription("Skip build :BRANCH_NAME: ${env.BRANCH_NAME} , Base_EI_Project: ${Base_EI_Project}");
	 
	//  }else{
     
	// stage("Checkout code: configuration"){
//         dir(base_ei_project_tem_path){

// try{
//            git credentialsId: 'devops_svc', url: "http://${Base_EI_Project}", branch: "${env.BRANCH_NAME}"
// }catch(err){
//     //  stage('Perpare Base EI') { 
//     //     echo "Trying to create a Base EI Branch With dev"
//     //     build job: 'integration/Integration Foundation/EI_MTP_Automation/MTP-Tools/Copy-Base-EI-Branch',parameters: [string(name: 'Base_EI_Project', value: Base_EI_Project),string(name: 'Source_Branch', value: 'dev'),string(name: 'Target_Branch', value: env.BRANCH_NAME) ], wait: true
//     // }
//         //  git credentialsId: 'devops_svc', url: "http://${Base_EI_Project}", branch: "${env.BRANCH_NAME}"
// }
            // def props1 = readYaml file: "jenkinsfile.pipeline.yml"
            // url = props1.url
            // ei_service_url=props1.ei_service_url
            // earth_service_check_url=props1.earth_service_check_url
            // earth_service_info_url=props1.earth_service_info_url
            // ei_service_related_info_html=" \n\nYour EI Service: ${ei_service_url}\n (Test Env can add a header skip-verify equaling true to skip verify)<text/>  \n\nBase EI Project: http://${Base_EI_Project} \n\nBase EI Project Logs: <a href='${url}'>url</a>  \n\n  Earth Service: ${earth_service_info_url} \n\nYour Commit Info: \n<text>${newCommitComments}</text>\n\nBuild Detail: ${env.BUILD_URL}console"
        
        // }  
    // }
// stage ('notify') {
//        emailext (
//                 // subject: "${build_email_title_suffix} CAR file Deploy to EI Server Successfully ", 
//                 subject: "CAR file Deploy to EI Server Successfully ", 
//                 mimetype: "text/html", 
//                 to: "mjavid.c@SPLonline.com.sa",
//                 // body: "${build_email_title_suffix} CAR file Deploy to EI Server Successfully \n\n All Environment Base EI Project Logs: \nCheck If zzzzzzz_MI_Health_Check Car Deployed : <a href='${Base_EI_Project} zzzzzzz_MI_Health_Check '></a> \n\nBase EI Project: http://${Base_EI_Project} \n \n\nYour Commit Info: \n<text>${newCommitComments}</text>\n\nBuild Detail: ${env.BUILD_URL}console \n \n In this Base EI Project version , We can not check whether the EI Service was deployed to Earth , and whether  there is other EI Service deployed failed make your EI Service unable to be deployed.\n\nif you need response your EI Service deployment status when you deploy ,  you can contact with SPL Integration Basis to upgrade the Base EI Project to release/v1.1 .  \n SPL Integration Basis: - "
//                 body: "CAR has deployed successfully"
//              ) 
//     }
    stage('Replace EI Serive Parameters') {
        dir ("${ei_service_project_tem_path}") {          
              dir ("SPLCICD/SPLDemo"){
                echo "Enter1"
                bat 'dir'                
                echo "enter2"
                props.environments.main.each {
                    echo "enter3"
                updateProperty("{${it.key}}", "${it.value}" ,'*/src/main/synapse-config/*/*.xml')
                // updateProperty("${it.key}", "${it.value}" ,'*/dataservice/*.dbs')
            }
              }
               
            
        }
    }
        def build_result = ""
        def deploy_result = ""
        bat "java -version"
        stage("Build in Maven"){
            echo "Enter Build in Maven"
            echo "cd ${ei_service_project_tem_path}"
          build_result = bat (returnStdout: true, script: "cd ${ei_service_project_tem_path}/SPLCICD/SPLDemo && mvn clean install -Dmaven.test.skip=true || echo success")
          is_car_build_error = build_result.contains("[ERROR]")
          echo "Exit Build in Maven"
          
        }

        stage("check user"){
wrap([$class: 'BuildUser']) {
   def user = env.BUILD_USER_ID
   echo "user logged in is=${user}"
   if("${user}"=="admin")
   {}
   else
   { 
    stage('QA Team') {
        echo "Build number is ${currentBuild.number}"
            emailext (
                subject: "Need Appproval for build",
                mimetype: 'text/html', 
                to: "${notify_users}",
                body: "build number" +"${currentBuild.number}"+"is pending for approval"
				)
          input message:"Need for admin approval", submitter: 'admin'
  
   
    
  }
   
   }
}
 }


        
        if(is_car_build_error){
        currentBuild.result = 'FAILURE'
        currentBuild.setDescription("${build_email_title_suffix} EI Car Build Error")
		    emailext (
            subject: "${build_email_title_suffix} EI Car Build Error", 
            mimetype: 'text/html', 
            to: notify_users,
            body: build_result
            ) 
		}else{

        stage("Cope car"){
             echo "Enter Cope car"
            echo "print base mi proj"
            echo "${ei_service_project_tem_path}"
        //  deploy_result =  
         powershell "copy ${ei_service_project_tem_path}/SPLCICD/SPLDemo/SPLDemoCompositeExporter/target/*.car C:/JenkinsShare"
        powershell "cd C:/temp; remove-item *.car"
        powershell "copy ${ei_service_project_tem_path}/SPLCICD/SPLDemo/SPLDemoCompositeExporter/target/*.car  C:/temp"
bat 'dir'
echo "*************neter***************"
def d="${currentBuild.number}"
echo "${env:BUILD_NUMBER}"
def query='get-childitem -Path "C:/temp" | where-object { $_.Name -like "*.car" } | %{ rename-item -LiteralPath $_.FullName -NewName '+"${currentBuild.number}"+'_$($_.name)}'
echo "${query}"
powershell 'get-childitem -Path "C:/temp" | where-object { $_.Name -like "*.car" } | %{ rename-item -LiteralPath $_.FullName -NewName '+"${currentBuild.number}"+'_$($_.name)}'

 //powershell "cd ei_service_project_tem_path/SPLCICD/SPLReleases; remove-item *.car"
 powershell "copy C:/temp/*.car ei_service_project_tem_path/SPLCICD/SPLReleases"
 echo "Exit Cope car"

  //powershell "Copy-Item -Path C:/JenkinsShare/SPLDemoCompositeExporter_1.0.0.car -Destination C:/temp"
        }

stage("backup car && commit to azure repo "){
        
         
bat 'dir'
echo 'cd ei_service_project_tem_path/SPLCICD/SPLReleases &&  git add . && git commit -m ${newCommitComments}  \n Build Detail: ${env.BUILD_URL}console && git push origin HEAD:main || echo success'
build_result1 = powershell (returnStdout: true, script: "cd ei_service_project_tem_path/SPLCICD/SPLReleases; git add . ; git commit -m 'commit'; git push origin HEAD:main ;echo success")
echo "${build_result1}"

//bat "cd ei_service_project_tem_path/SPLCICD/SPLReleases"
bat 'dir'
             //   powershell 'git config --global user.email "wso2devops@devopsadmin.com"'
               // powershell 'git config --global user.name "wso2devops"'   
                //powershell 'ls'
               // powershell 'git add .'
                //powershell "git commit -m '${newCommitComments}  \n Build Detail: ${env.BUILD_URL}console'"
               // powershell "git push origin HEAD:main"
                currentBuild.setDescription("Success deploy : ${newCommitComments}");
        }
        
    }
  
        // stage("Deploy car in server"){
        //     echo "deploy car in server"
        //     deploy_result = bat (returnStdout: true, script: "cd ${ei_service_project_tem_path}/SPLDemo && mvn  deploy -Dmaven.deploy.skip=true -Dmaven.car.deploy.skip=false -Dmaven.test.skip=true -Dmaven.wagon.http.ssl.insecure=true || echo success")
        //      is_car_deploy_error = deploy_result.contains("[ERROR]")

        // }
        // if(is_car_deploy_error){
        // currentBuild.result = 'FAILURE'
        // currentBuild.setDescription("${build_email_title_suffix} EI Car Deploy Error")
		//     emailext (
        //     subject: "${build_email_title_suffix} EI Car Deploy Error", 
        //     mimetype: 'text/html', 
        //     to: notify_users,
        //     body: deploy_result
        //     ) 
		// }
        
        // stage("Record Commit ID"){
        //       def template_path= "${base_ei_project_tem_path}/repository/deployment/server/synapse-configs/default/templates/"

        //       if(fileExists("${template_path}EI_Service_CommitID_Recorder.xml")){
        //           commit_id_exist = true;
        //           def commit_id_list_string = bat (returnStdout: true, script: "type ${template_path}EI_Service_CommitID_Recorder.xml|FIND -oE 'commit_id_start.*commit_id_end'")
        //           def new_commit_id_list_string = "";
        //           def commit_id_list = commit_id_list_string.split(',');
        //           commit_id_list.eachWithIndex { item, i ->
        //              if(i==0){
        //                 new_commit_id_list_string = item.trim()+","+commit_id.trim();
        //              }else
        //              if(i<20){
        //                   new_commit_id_list_string+=","+item.trim();
        //              }
        //           }
        //           new_commit_id_list_string+=",commit_id_end";

                
        //           echo "commit_id_list_string=${commit_id_list_string}"
        //           echo "new_commit_id_list_string=${new_commit_id_list_string}"
        //           def sed_command = "sed -i 's|${commit_id_list_string.trim()}|${new_commit_id_list_string.trim()}|g' ${template_path}EI_Service_CommitID_Recorder.xml"
        //           echo "sed_command=${sed_command}"
        //           bat "${sed_command}"
        //       }
        // }        
//       		}         
    // }
    
   if(!is_car_build_error){
    // stage("commit & push code to Base_EI_Project"){
    //     dir(base_ei_project_tem_path){
    //          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'devops_svc',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]){
    //             powershell 'git config --global user.email "wso2devops@devopsadmin.com"'
    //             powershell 'git config --global user.name "wso2devops"'
    //             powershell 'git add --all'
    //             powershell "git commit -m '${newCommitComments}  \n Build Detail: ${env.BUILD_URL}console'"
    //             powershell "git push http://$USERNAME:$PASSWORD@${Base_EI_Project} ${env.BRANCH_NAME}"
    //             currentBuild.setDescription("Success deploy : ${newCommitComments}");
    //     }
    //     }
    // }


	// echo "earth_service_check_url = ${earth_service_check_url} ,commit_id_exist = ${commit_id_exist}"
    // if(is_car_deploy_error){
	//     stage("Wait for Earth Deployment"){
    //     project_name = gitProjectName.split('/')[1];
    //     try {
    //         timeout(time: 180, unit: 'SECONDS') {
    //             sleep 20
	// 			waitUntil {
	// 			     earth_service_check_url = earth_service_check_url.replaceAll("service_commit_id",commit_id.trim())
    //                  echo "earth_service_check_url=${earth_service_check_url}"
    //                  def key_words = 'Commit ID Exist'
    //                  def earth_ei_service_response = bat(returnStdout: true, script: "${earth_service_check_url} || echo success")
    //                  echo "earth_ei_service_response=${earth_ei_service_response}"
	// 				 if(earth_ei_service_response.contains(key_words)){
	// 				  return true;
	// 				 }
	// 				 return false;
	// 			}
    //             emailext (
    //             subject: "${build_email_title_suffix} EI Car Deploy Successfully", 
    //             mimetype: 'text/html', 
    //             to: notify_users,
    //             body: "You can access your service now. ${ei_service_related_info_html}"
    //          ) 
	// 		}
    //         }catch (err) {
    //     currentBuild.result = 'FAILURE'
    //     currentBuild.setDescription("Wait for Earth Deployment Failed :RANCH_NAME: ${env.BRANCH_NAME} , Base_EI_Project: ${Base_EI_Project}")

    //         emailext (
    //         subject: "${build_email_title_suffix} EI Car Deploy Failedd", 
    //         mimetype: 'text/html', 
    //         to: notify_users,
    //         body: "${build_email_title_suffix} deploy failed , is car <a href='${url} Ignoring Carbon Application'> deployment error </a>?   \n\nError: ${err} \n\n ${ei_service_related_info_html}  \n\n if can not find the RC , please contact with SPL Integration Basis .\n\nEmail: - \n\n Detail: ${env.BUILD_URL}console ",
    //          ) 
        
    // }
         
    //         }
//  }
//  else{
echo "ENter Email"
 echo "Base_EI_Project = ${Base_EI_Project}"
    echo "notify_users = ${notify_users}"
    echo "${build_email_title_suffix} CAR file Deploy to EI Server Successfully "
    echo "Base_EI_Project ${Base_EI_Project}"
    echo "newCommitComments ${newCommitComments}"
    echo "BUILD_URL ${env.BUILD_URL}"
    stage ('notify') {
       emailext (
                subject: "${build_email_title_suffix} CAR file Deploy to EI Server Successfully ", 
                // subject: "CAR file Deploy to EI Server Successfully ", 
                mimetype: 'text/html', 
                to: "${notify_users}",
                body: "${build_email_title_suffix} CAR file Deploy to EI Server Successfully \n\n All Environment Base EI Project Logs: \n Check If EI_Health_Check Car Deployed : <a href='${Base_EI_Project} EI_Health_Check '></a> \n\nBase EI Project: http://${Base_EI_Project} \n \n\nYour Commit Info: \n<text>${newCommitComments}</text>\n\nBuild Detail: ${env.BUILD_URL}console \n \n In this Base EI Project version , We can not check whether there is other EI Service deployed failed make your EI Service unable to be deployed.\n\nif you need response your EI Service deployment status when you deploy , you can contact with SPL Integration Basis to upgrade the Base EI Project to release/v1.1 .  \n SPL Integration Basis: - \n\n  "
                // body: 'CAR has deployed successfully'
             ) 
    }
    echo "Exit email"
 }
//   }
    // }
  } catch (err) {
        currentBuild.result = 'FAILURE'
        def to_users = notify_users;
        
        if(!to_users){
            to_users =  env.Notify_Users;
        }
        stage ('notify') {
            emailext to: "${to_users}",
            recipientProviders: [[$class: 'RequesterRecipientProvider'],[$class: 'DevelopersRecipientProvider']],
            subject: "${build_email_title_suffix} EI Car Deploy Failed",
            body: "${build_email_title_suffix} deploy failed \n\nError: ${err} \n\n${ei_service_related_info_html} \n\n if can not find the RC , please contact with SPL Integration Basis .\n\nEmail: - ",
            mimeType: 'text/html'
        }
    }
}

def concatNotifyUsers(userList1,userList2){

        if(userList1 && userList2){
            return userList1+','+userList2;
        }
        
        if(userList1){
           return userList1;
        }

        if(userList2){
           return userList2;
        }

        return 'no-reply@splonline.com.sa';
}
def updateProperty(property, value, file) { 
     echo 'Inside updateProperty method..'
         echo "file name: $file + Property: $property  value: $value"

        escapedProperty = property.replace('[', '\\[').replace(']', '\\]').replace('.', '\\.')
        echo "enter4"
        echo "file name: $file + escapedProperty: $escapedProperty  value: $value"      
      def updateStatus = powershell (returnStdout: true, script: "Set-Alias -Name sed -Value C:/'Program Files'/Git/usr/bin/sed.exe; sed -i 's|$escapedProperty|$value|g' $file | echo success")

echo "enter5"
// def stdout1 = powershell(returnStdout: true, script: """
// get-content $file """ + ' | %{$_ -replace '+ """ $escapedProperty,"http://100.67.10.91:9763/services/Moto_eCommerce_Delivered_Orders_DataService" }
// """

// )
//     println stdout1

   
        // echo "sed -i 's|$escapedProperty|$value|g' $file"
        
        
        //  powershell ("get-content */src/main/synapse-config/*/*.xml | %{$_ -replace 'WMSLITE_OPS_SHIPMENT_DSS_EP','http://100.67.10.91:9763/services/Moto_eCommerce_Delivered_Orders_DataService'}")
        
    //    def updateStatus = powershell (returnStdout: true, script: "get-content */src/main/synapse-config/*/WMSLite_OPS_Shipment_DSS_EP.xml | %{$_ -replace "WMSLITE_OPS_SHIPMENT_DSS_EP","http://100.67.10.91:9763/services/Moto_eCommerce_Delivered_Orders_DataService"})
    //   def updateStatus = powershell (returnStdout: true, script: "get-content $file | %{$_ -replace $escapedProperty,$value}")

       println updateStatus
}

// def parseUrl(Base_EI_Project){
//     echo  "********Enter*********"
//     // def base_ei_project_name = Base_EI_Project.split('openesb-image-config/')[1].split('.git')[0]
//     def base_ei_project_name = Base_EI_Project
//     echo "base_ei_project_name ${base_ei_project_name}"
//     def env = env.BRANCH_NAME == 'main'?'PROD':env.BRANCH_NAME;
//    return " ${base_ei_project_name} AND ${env}";
// }

def getNotNullValue(value, defauleValue) { 
      if(value){
          return value;
      }
      return defauleValue;
}

