{
    "recordTypeMap": {},
    "RecordSetBundles": [
        {
            "Records": [
                {
                    "attributes": {
                        "type": "copado__Function__c",
                        "url": "/services/data/v52.0/sobjects/copado__Function__c/a0k09000000FKgfAAG"
                    },
                    "copado__API_Name__c": "Devhub_Package_Info",
                    "copado__Description__c": "Extracts package and package versions info from a DevHub",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"PACKAGE\"\n}, {\n  \"required\" : true,\n  \"name\" : \"SESSION\",\n  \"defaultValue\" : \"{$Source.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"BASEURL\",\n  \"defaultValue\" : \"{$Source.Credential.Endpoint}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n\nconst execSync = require(\"child_process\").execSync;\n\nconst url = process.env['BASEURL'];\nconst baseurl = url.substring(0, url.indexOf(\"/\", url.indexOf(\"/\") + 2));\nconst session = process.env['SESSION'];\nconst pkgnameorId = process.env['PACKAGE'];\n\nlet resultObj = '\\'No results found\\'';\n\nconst setup = `\n\tcopado -p 'Setting instance url'\n    sfdx force:config:set instanceUrl=${baseurl} --global\n    \n    copado -p 'Creating Temp Project'\n    sfdx force:project:create -n temp\n    \n    copado -p 'Getting Package Info'\n`;\nexecSync(setup);\n\nconst listPackages = `    \n    cd temp\n    sfdx force:package:list -v ${session} --json\n`;\nconst packages = JSON.parse(execSync(listPackages).toString());\nconst pkg = packages.result.find(\n  (element) => element.Name == pkgnameorId || element.Id == pkgnameorId\n);\n\nif(pkg) {\n  const listVersions = `\n      cd temp\n      sfdx force:package:version:list -v ${session} -p ${pkg.Id} --json\n  `;\n\n  const versions = JSON.parse(execSync(listVersions).toString());\n\n  resultObj = JSON.stringify({ pkg: pkg, versions: versions.result });\n}\n\nconst result = `\n\tcopado -p 'Updating Results' -r '${resultObj}'\n`;\nexecSync(result);",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1.0",
                    "copado__Worker_Size__c": "S",
                    "CreatedDate": "2021-09-16T15:20:01.000+0000",
                    "Id": "a0k09000000FKgfAAG",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-09-28T13:50:51.000+0000",
                    "LastReferencedDate": "2021-09-29T14:36:54.000+0000",
                    "LastViewedDate": "2021-09-29T14:36:54.000+0000",
                    "Name": "Devhub Package Info",
                    "SystemModstamp": "2021-09-28T13:50:51.000+0000"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0l7Q000000iAiJQAU"
                    },
                    "copado__API_Name__c": "sfdx_install_package",
                    "copado__Callback_Type__c": "Flow",
                    "copado__FlowHandler__c": "cmcSf.Installed_Package_Callback",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"packages\",\n  \"defaultValue\" : \"{$Job.PrevStep.Result__r.Result_Data__c}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"baseUrl\",\n  \"defaultValue\" : \"{$Destination.Credential.Endpoint}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"session\",\n  \"defaultValue\" : \"{$Destination.Credential.SessionId}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"wait\",\n  \"defaultValue\" : \"200\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n\nconst { execSync } = require(\"child_process\"),\n    { packages, baseUrl, session, wait } = process.env,\n    url = baseUrl.substring(0, baseUrl.indexOf(\"/\", baseUrl.indexOf(\"/\") + 2));\n\n// EXECUTION\n\nexecute();\n\nfunction execute() {\n    try {\n        setInstanceURL();\n        installPackages();\n    }\n    catch(error) {\n        execSync(showErrorCmd(error.toString()));\n    }\n}\n\n// SCRIPT FUNCTIONS\n\nfunction setInstanceURL() {\n    execSync(`\n        copado -p 'Setting instance url'\n        sfdx force:config:set instanceUrl=${url} --global || (${showErrorCmd(`Error setting instance URL`)})\n    `);\n}\n\nfunction installPackages() {\n    const listOfPackages = JSON.parse(packages);\n    if(!listOfPackages?.length) {\n        throw 'Packages list to install is empty.';\n    }\n    listOfPackages.forEach(pkg => {\n        if(pkg.id) {\n            executePackageInstallCmd(pkg);\n        }\n    });\n}\n\nfunction executePackageInstallCmd(package) {\n    let params = `-u ${session} -p ${package.id} -w ${wait}`;\n    params += package.key ? ` -k ${package.key}` : '';\n    \n    const response = JSON.parse(execSync(`\n        copado -p 'Installing package ${package.id}'\n        sfdx force:package:install ${params} --json || true\n    `).toString());\n\n    if(response?.status) {\n        throw `${response.name}: ${response.message}`;\n    }\n    execSync(`copado -p 'Package \"${package.id}\" successfully installed'`);\n}\n\nfunction showErrorCmd(error) {\n    const suffix = \"Please check the logs for details\";\n    return `copado -p 'Error' -e \"${error}, ${suffix}\" && exit 1`;\n}",
                    "copado__Timeout__c": 200,
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1.0",
                    "copado__Worker_Size__c": "S",
                    "Id": "a0l7Q000000iAiJQAU",
                    "LastReferencedDate": "2022-07-05T10:08:24.000+0000",
                    "LastViewedDate": "2022-07-05T10:08:24.000+0000",
                    "Name": "Sfdx Install Package"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0l7Q000000MDYvQAO"
                    },
                    "copado__API_Name__c": "sfdx_deploy",
                    "copado__Callback_Type__c": "Flow",
                    "copado__Description__c": "Deploys promotion branch",
                    "copado__FlowHandler__c": "cmcSf.Update_Validation_Id_on_Promotion",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"file_changes_id\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.fileWithSelectedChanges}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"promotion\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.promotionBranchName}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"target_branch\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.destinationBranchName}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"destination_instance_url\",\n  \"defaultValue\" : \"{$Destination.Credential.Endpoint}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"destination_sessionid\",\n  \"defaultValue\" : \"{$Destination.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"git_json\",\n  \"defaultValue\" : \"{$Context.Repository.Credential}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"destination_env\",\n  \"defaultValue\" : \"{$Destination.apex.EnvironmentVariables}\"\n}, {\n  \"name\" : \"find_and_replace_rules\",\n  \"defaultValue\" : \"{$Context.apex.GlobalFindAndReplaceDestinationId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"conflict_resolution_attachments\",\n  \"defaultValue\" : \"{$Context.apex.GetConflictResolutionAttachments}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"deploy_id\",\n  \"defaultValue\" : \"{$Job.ExecutionParent.Id}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"isValidation\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.deploymentDryRun}\"\n}, {\n  \"name\" : \"git_name\",\n  \"defaultValue\" : \"{$User.Name}\"\n}, {\n  \"name\" : \"git_email\",\n  \"defaultValue\" : \"{$User.Email}\"\n}, {\n  \"name\" : \"repository_id\",\n  \"defaultValue\" : \"{$Pipeline.Git_Repository__r.Id}\"\n}, {\n  \"name\" : \"testLevel\",\n  \"defaultValue\" : \"{$Job.ExecutionParent.Promotion__r.cmcSf__Apex_Test_Level__c}\"\n}, {\n  \"name\" : \"testOnlyClasses\",\n  \"defaultValue\" : \"{$Context.apex.cmcSf.GetTestOnlyClasses}\"\n}, {\n  \"name\" : \"debugMode\",\n  \"defaultValue\" : \"{$Job.ExecutionParent.Promotion__r.cmcSf__Debug_Mode__c}\"\n}, {\n  \"name\" : \"git_depth\",\n  \"defaultValue\" : \"100\"\n}, {\n  \"name\" : \"validationId\",\n  \"defaultValue\" : \"{$Job.ExecutionParent.Promotion__r.cmcSf__Validate_Deploy_Request_Id__c}\"\n}, {\n  \"name\" : \"testClasses\",\n  \"defaultValue\" : \"{$Context.apex.cmcSf.GetTestClasses}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n/**\n * Performs deploy of selected user story metadata changes.\n * Returns (If ACTION success) destination branch with merged changes of the user story metadata\n * (If ACTION failed) Returns details with error status on the job execution\n * @param file_changes_id\n * @param promotion\n * @param target_branch\n * @param destination_instance_url\n * @param destination_sessionid\n * @param git_json\n * @param destination_env\n * @param find_and_replace_rules\n * @param conflict_resolution_attachments\n * @param deploy_id\n * @param isValidation\n * @param git_name\n * @param git_email\n * @param repository_id\n * @param testLevel\n * @param testClasses\n * @param testOnlyClasses\n*/\n\nconst { execSync } = require('child_process'),\n    { writeFileSync, existsSync, readFileSync, rmSync, statSync } = require('fs'),\n    { env, chdir } = require('process'),\n    sourceApiVersion = \"55.0\",\n    encodedChangesFile = 'encoded_changes.json',\n    { destination_sessionid, promotion, target_branch, isValidation } = env,\n    targetDirectory = \"/app/repository\",\n    ACTIONS = {\n        ADD: 'add',\n        RETRIEVE_ONLY: 'retrieveonly',\n        FULL: 'full',\n        DELETE: 'delete'\n    },\n    TYPES = {\n        PERMISSION_SET: 'permissionset',\n        PROFILE: 'profile'\n    };\n\nlet commitedMetadata = {};\n\n// EXECUTION\n\nexecute();\n\nfunction execute() {\n    const { debugMode, target_branch, validationId } = env;\n    let debugBranch;\n    try {\n        copadoPrepareMetadataChangeslList();\n        getPromotionAndConfigureGit();\n        gitMergePromotionToTarget();\n        checkSFDXProjectJson();\n        setSFDXConfig();\n        reconcileFullProfile();\n        if(debugMode === \"true\") {\n            debugBranch = `copadoDebug/${promotion?.split('/')[1]}`;\n            createDebugPromotionBranch(debugBranch);\n        }\n        enrichChangeListAndProcessMetadata();\n        replaceEnvironmentVariablesAndReplaceYAML();\n        if(debugMode === \"true\") {\n            pushDebugPromotionBranch(debugBranch);\n        }\n        if(isValidation === \"false\" && validationId) {\n            executeQuickDeploy(validationId);\n        }\n        else {\n            handleDeployResult(copadoStartDeployment());\n        }\n        cleanSfdxProjectConfigFiles();\n        if(isValidation === \"false\") {   \n            pushChangesToRemote(target_branch);\n        }\n    } catch (err) {\n        showError(err.toString());\n    }\n}\n\n// SCRIPT FUNCTIONS\n\nfunction copadoPrepareMetadataChangeslList() {\n    commitedMetadata.all = require(`/app/${encodedChangesFile}`);\n    commitedMetadata = {\n        ...commitedMetadata, \n       \tadd: commitedMetadata.all.filter(selectedMetadata => ((selectedMetadata.a.toLowerCase() == ACTIONS.ADD) || ( selectedMetadata.a.toLowerCase() == ACTIONS.FULL && selectedMetadata.t.toLowerCase() == TYPES.PERMISSION_SET ))),\n        delete: commitedMetadata.all.filter(selectedMetadata => selectedMetadata.a.toLowerCase() == ACTIONS.DELETE),\n        profiles: commitedMetadata.all.filter(selectedMetadata => selectedMetadata.a.toLowerCase() == ACTIONS.FULL && selectedMetadata.t.toLowerCase() == TYPES.PROFILE),\n        getMetadataList(type) {\n            return this[type].map(step => step.t + ':' + step.n).toString(',');\n        }\n    }\n}\n\nfunction getPromotionAndConfigureGit() {\n    const {\n        git_email,\n        git_name\n    } = env;\n\n        execSync(`\n            sfdx -v\n            # Get Promotion\n            mkdir -p ${targetDirectory}\n            cd ${targetDirectory}\n            copado --progress \"fetching ${promotion}\"\n            copado-git-get \"${promotion}\" || ${getErrorCmdString('Error fetching promotion branch')}\n\n            # Configure Git\n            git config --local user.email \"${git_email}\" || exit 1\n            git config --local user.name \"${git_name}\" || exit 1\n            git config --local diff.renames false || exit 1\n            git config --local merge.renames false || exit 1\n            git config --local status.renames false || exit 1\n\n        `, {\n            stdio: 'inherit'\n        });\n        chdir(targetDirectory);\n}\n\nfunction checkSFDXProjectJson() {\n    const sfdxProjectJsonPath = `${targetDirectory}/sfdx-project.json`;\n    if(existsSync(sfdxProjectJsonPath)) {\n        let fileContent = JSON.parse(readFileSync(sfdxProjectJsonPath, { encoding:'utf8' }));\n        if(fileContent.sourceApiVersion !== sourceApiVersion) {\n            const commitMessage = `Updated  sourceApiVersion from ${fileContent.sourceApiVersion} to ${sourceApiVersion} in sfdx-project.json to align the commit, promote and deploy operations with the latest supported api version of Copado.`;\n            fileContent.sourceApiVersion = sourceApiVersion;\n            writeFileSync(sfdxProjectJsonPath, JSON.stringify(fileContent, null, 2));\n            commitGit(commitMessage); \n        }\n    } else {\n        throw \"Invalid configuration. sfdx-project.json is invalid or missing at project root. Copado Commit and Deploy operations are required to run from within a valid sfdx project.\";\n    }\n}\n\nfunction enrichChangeList(filePath) {\n    execSync(`enricher -p ${filePath} --repo ${targetDirectory}/ || ${getErrorCmdString(\"Error processing metadata\")}`, { stdio: 'inherit'});\n}\n\nfunction enrichChangeListAndProcessMetadata() {\n    enrichChangeList(`/app/${encodedChangesFile}`);\n    execSync(`\n    # Metadata Processor service called\n    copado -p \"Processing metadata\"\n    enricher -p /app/${encodedChangesFile} --repo ${targetDirectory}/ || ${getErrorCmdString(\"Error processing metadata\")}\n    metadata-processor \"/app/${encodedChangesFile}\" \"${targetDirectory}\" -o TRIM || ${getErrorCmdString(\"Error processing metadata\")}`, {\n        stdio: 'inherit' \n    });\n    if(existsSync(`/app/internal_${encodedChangesFile}`)) {\n        addParentMetadataToOriginalChangeList(require(`/app/internal_${encodedChangesFile}`));\n    }\n}\nfunction addParentMetadataToOriginalChangeList(parentMetadataList) {\n    commitedMetadata.add = commitedMetadata.add.concat(parentMetadataList.filter(change => change.a.toLowerCase() == ACTIONS.ADD));\n}\n\n\nfunction varReplace() {\n    const filesToIncludeListPath = '/tmp/filesToInclude.json',\n        { destination_env } = env;\n\n    writeFileSync(filesToIncludeListPath, JSON.stringify(getFilePaths(`/app/${encodedChangesFile}`, [ ACTIONS.ADD, ACTIONS.FULL ] )));\n\n    const varreplace = `\n    copado -p \"Replacing environment variables, if any\"\n    varreplace '${destination_env}' '${targetDirectory}' --include=${filesToIncludeListPath} || ${getErrorCmdString(\"Error replacing environment variables\")}`;\n\n    execSync(varreplace, {stdio: 'inherit'});\n}\n\nfunction replaceEnvironmentVariablesAndReplaceYAML() {\n    const {\n        find_and_replace_rules\n    } = env;\n\n    if(commitedMetadata.add?.length > 0 || commitedMetadata.profiles?.length > 0) {\n        varReplace();\n\n        execSync(`\n            copado -p \"Applying global find and replace rules, if any\"\n            if [ -z \"${find_and_replace_rules}\" ]\n            then\n                echo \"No yaml file found\"\n            else\n                copado --downloadfiles \"${find_and_replace_rules}\" --downloaddir /tmp/ || ${getErrorCmdString(\"Error downloading Copado.yml file\")}\n\n                # YAML Replace service called, Target Dir:  \"/tmp/Copado\", Path to YML: \"/app/repository/\"\n                yamlreplace \"/tmp/Copado\" \"${targetDirectory}\" -b \"${promotion}\" ||  ${getErrorCmdString(\"Error applying global find and replace rules\")}\n            fi\n        `, {\n            stdio: 'inherit'\n        });\n    }  \n}\n \nfunction copadoStartDeployment() {\n\tconst addManifest = commitedMetadata.profiles?.concat(commitedMetadata.add).filter(meta => meta != undefined);\n    let manifestCreation = `\n        copado --progress 'Deploying ${commitedMetadata.getMetadataList(ACTIONS.ADD).substring(0,240)}...'\n        sfdx force:source:manifest:create -m '${addManifest.length ? addManifest.map(step => step.t + \":\" + step.n).toString() : \"\"}'\n    `;\n    commitedMetadata.delete.length && (manifestCreation += `\\nsfdx force:source:manifest:create -m '${commitedMetadata.getMetadataList('delete')}' --manifesttype destroy`);\n\n    execSync(manifestCreation, {\n        stdio: 'inherit'\n    });\n    return executeSFDXcommand(`(${createSfdxDeploy()}) || true`);\n    \n}\n\nfunction executeSFDXcommand(command) {\n    return JSON.parse(execSync(command).toString());\n}\n\n\n// Create SFDX Deploy command w.r.t. to flags\nfunction createSfdxDeploy() {\n    let sfdxDeployCommand = \"sfdx force:source:deploy --json\";\n    \n    if(isValidation === 'true') {\n        sfdxDeployCommand += \" --checkonly\";\n    }\n    sfdxDeployCommand += ` --targetusername ${destination_sessionid}`;\n\n    sfdxDeployCommand += getTestRunParam();\n\n    sfdxDeployCommand += \" --manifest package.xml\" + (commitedMetadata.delete.length ? \" --postdestructivechanges destructiveChanges.xml -g\" : \"\");\n    console.log(`Deploy command: ${sfdxDeployCommand}`);\n    return `${sfdxDeployCommand} || true`;\n}\nfunction getTestRunParam() {\n    let testRunParam = '';\n    const { testLevel } = env;\n    if(testLevel) {\n        const testRun = testLevel?.split(' ')?.join('');\n        if (testRun) {\n            testRunParam += ` -l ${testRun}`;\n            if(testRun === \"RunSpecifiedTests\") {\n                testRunParam += `${ \" -r \" + getDeploymentTestClasses()}`\n            }\n        }\n    }\n    return testRunParam;\n}\n\nfunction getDeploymentTestClasses() {\n   const { testOnlyClasses, testClasses } = env;\n   let deploymentTestClasses = new Set();\n    if(testOnlyClasses) {\n        deploymentTestClasses = new Set([...deploymentTestClasses, ...(testOnlyClasses?.split(','))]);\n    }\n    if(testClasses) {\n        deploymentTestClasses = new Set([...deploymentTestClasses, ...(testClasses?.split(','))]);\n    }\n    if(!deploymentTestClasses.size) {\n        throw \"No Test Classes were selected by the User\";\n    }\n    return [...deploymentTestClasses].join(',');\n}\n\nfunction uploadDeploymentResultOnJobStep(deployResult) {\n        deployResultDir = `/app/${isValidation === \"true\" ? 'Validation':'Deployment'}Result.json`;\n\n    writeFileSync(deployResultDir, JSON.stringify(deployResult, null, 2));\n    execSync(`copado --uploadfile ${deployResultDir}`, {\n       stdio: 'inherit'\n   });\n}\n\nfunction handleDeployResult(deployResult) {\n\n    uploadDeploymentResultOnJobStep(deployResult);\n\n    let errorResponse = \"\";\n    const deployResultDetails = deployResult?.result?.details,\n       failedMetadata = deployResultDetails?.componentFailures,\n       warningSuccessMetadata = deployResultDetails?.componentSuccesses,\n       failedTests = deployResultDetails?.runTestResult?.failures,\n       codeCoverageWarnings = deployResultDetails?.runTestResult?.codeCoverageWarnings,\n       filterFailedMedataProblemType = (statusInfo, type) => {\n           return Array.isArray(statusInfo) ? statusInfo.filter(status => status.problemType == type) : (  statusInfo.problemType == type ? [statusInfo] : []);\n       },\n       getFailedMedataErrorMessage = (failedTests) => {\n           return Array.isArray(failedTests) ? failedTests.map(fail => (`${fail.name}-${fail.methodName ? fail.methodName+\"-\" : \"\"}${fail.message}`)).join(\"\\n\") : `${typeof failedTests.name == 'object' ? \"\" : failedTests.name+\"-\"}${failedTests.methodName ? failedTests.methodName+\"-\" : \"\"}${failedTests.message}`;\n       },\n       getFailedDeploymentMessage = (problem) => {\n           return Array.isArray(problem) ? problem.map(fail => fail['componentType'].concat(':', fail['fullName'], ':', fail['problem'])).join(\"\\n\") : problem['componentType'].concat(':', problem['fullName'], ':', problem['problem']).toString()\n       }, \n       populateErrorResponse = (errorMessage) => {\n           const delimeter = \"\\n\";\n           errorResponse = (errorResponse && errorResponse.concat(`${delimeter}`, errorMessage)) || errorResponse.concat(errorMessage);\n       }\n\n   // If any errors or warning, display to progress indicator and fail the job execution\n   if (deployResult?.status) {\n        if(deployResult?.message) {\n            errorResponse = `${deployResult?.message}`;\n        }\n   }\n   if (failedTests) {\n       populateErrorResponse(getFailedMedataErrorMessage(failedTests));\n      }\n   if (codeCoverageWarnings) {\n       populateErrorResponse(getFailedMedataErrorMessage(codeCoverageWarnings));\n   }\n   if (failedMetadata) {\n       const errorMetadata = filterFailedMedataProblemType(failedMetadata, \"Error\");\n       errorMetadata.length && populateErrorResponse(getFailedDeploymentMessage(errorMetadata));\n   }\n   if(warningSuccessMetadata?.length) {\n       const warningMetadata = filterFailedMedataProblemType(warningSuccessMetadata, \"Warning\");\n       if(warningMetadata?.length) {\n           message = `WARNING deploying Metadata: ${getFailedDeploymentMessage(warningMetadata)}`;\n           execSync(`copado -p \"${message.substring(0,254)}\"`, { stdio: 'inherit' });\n           console.log(message);\n       }\n   }\n   showError(errorResponse.substring(0,131072));\n\n   if(!deployResult.status && isValidation === \"true\") {\n       populateValidationIdOnResultRecord(deployResult.result.id);\n   }\n}\n\nfunction showError(errorResponse) {\n    errorResponse && execSync(`copado -p \"Error\" -e \"${errorResponse}\" && exit 1`, {\n        stdio: 'inherit'\n    });\n}\nfunction cleanSfdxProjectConfigFiles() {\n    const cleanUp = `\n        git reset --hard\n        rm -rf package.xml destructiveChanges.xml\n    `;\n    execSync(cleanUp, {\n        stdio: 'inherit'\n    });\n}\n\nfunction gitMergePromotionToTarget() {\n    const { git_depth } = env;\n \n    let readyToMerge = `\n        git reset --hard || exit 1\n        copado --progress \"merging ${promotion} into ${target_branch}\"\n        copado-git-get --depth ${git_depth} \"${target_branch}\" ||  ${getErrorCmdString('Error fetching target branch')}\n        git merge \"${promotion}\"  --no-commit -Xignore-space-change || true\n    `;\n    execSync(readyToMerge, {\n        stdio: 'inherit'\n    });\n\n    evaluateMergeStatus();\n    commitGit(`Merging ${promotion} into ${target_branch} after auto conflict resolution`);\n}\n\nfunction commitGit(commitMessage) {\n    execSync(`           \n        git add . # add all the resolved changes, if any \n        git commit -am \"${commitMessage}\" || true`, {\n        stdio: 'inherit'\n    });\n}\n\nfunction evaluateMergeStatus() {\n        gitStatus = execSync('git status --porcelain=v1 -uno').toString(),\n        porcelainStatus = gitStatus.split('\\n').map(str => str.split(' ')?.[0]),\n        isConflict = porcelainStatus.length ? ['AA', 'UU', 'DD', 'UA', 'UD', 'DU', 'AU'].some(s => porcelainStatus.includes(s)) : false;\n    let infoMessage;\n\n    if(isConflict)\n        showError(`Changes detected in target branch '${target_branch}' after promotion branch '${promotion}' was created, please recreate promotion branch out of the new target branch state.`);\n    else if(!gitStatus) {\n        infoMessage = 'Already up to date';\n    }\n    else {\n        infoMessage = `Changes detected in target branch '${target_branch}' after promotion branch '${promotion}' was created.`;\n       \n    }\n    infoMessage && execSync(`copado -p \"${infoMessage}\"`, {\n        stdio: 'inherit'\n    });\n    console.log(infoMessage);\n}\n\nfunction reconcileFullProfile() {\n    if (commitedMetadata?.profiles?.length) {\n        let reconciledProfileOutput = execSync(`\n        \t\tcopado -p \"Reconciling full profile\"\n                sfdx sfpowerkit:source:profile:reconcile -u \"${destination_sessionid}\" -n \"${commitedMetadata.profiles.map(step => step.n).toString(',')}\" --json || true\n        `).toString();\n\n        if (!reconciledProfileOutput) {\n            throw \"Error reconciling full profile\";\n        }\n        reconciledProfileOutput = JSON.parse(reconciledProfileOutput);\n        if (reconciledProfileOutput?.status && reconciledProfileOutput?.message) {\n            execSync(getErrorCmdString(`${reconciledProfileOutput.commandName + \": \" + reconciledProfileOutput.message}`));\n        }\n        rmSync(\"sfpowerkit-cache.db\", {\n            force: true,\n        });\n        commitGit(`Cleaned full profile in promotion ${promotion}`);\n    }\n}\n\nfunction pushChangesToRemote(remoteBranchName) {\n    execSync(`git push origin \"${remoteBranchName}\" --atomic || ${getErrorCmdString(\"Could not push the changes to \" +remoteBranchName)}`);\n}\n\nfunction createDebugPromotionBranch(debugBranch) {\n\n    execSync(`\n    exists=$(git show-ref refs/heads/${debugBranch})\n    if [ -n \"$exists\" ]; then\n        copado -p \"Removing local debug branch ${debugBranch}\"\n        git branch -d ${debugBranch}\n    fi\n    git checkout -b ${debugBranch}\n    ` , { stdio: 'inherit' });\n}\n\nfunction pushDebugPromotionBranch(debugBranch) {\n    execSync(`copado -p \"Pushing changes to ${debugBranch}\"`);\n    commitGit(`Copado Debug Branch Commit for Promotion ${debugBranch}`);\n    execSync(`git push -f origin ${debugBranch} || copado -p \"Could not push changes to ${debugBranch}\"`);\n}\n\nfunction setSFDXConfig() {\n    const { destination_instance_url } = env;\n    execSync(`\n    # Set instance URL within sfdx-config.json\n    sfdx config:set instanceUrl=${destination_instance_url.substring(0, destination_instance_url.indexOf('.com') + 4)}`);\n}\n\nfunction getErrorCmdString(error) {\n    const suffix = 'Please check the logs for details.'\n    return `{ copado -p \"Error\" -e \"${error}. ${suffix}\"; exit 1; }`\n}\n\nfunction populateValidationIdOnResultRecord(validationId) {\n   execSync(`copado -p \"Saving validation id\" -r '${JSON.stringify({validationId})}' && exit 0`, { stdio: 'inherit' });\n}\n\nfunction executeQuickDeploy(validationId) {\n    const quickDeployResult = executeSFDXcommand(`sfdx force:source:deploy --json -q ${validationId} -u ${destination_sessionid}|| true`);\n    if(quickDeployResult.status) {\n        execSync(`copado -p \"${quickDeployResult.message}\"`);\n        handleDeployResult(copadoStartDeployment());\n    }\n    else {\n        uploadDeploymentResultOnJobStep(quickDeployResult);\n        execSync(`copado -p \"Quick Deploy done successfully\"`);\n    }\n}\n\nfunction readFromPath(filePath) {\n    return JSON.parse(readFileSync(filePath, 'utf-8'));\n}\n\nfunction getFilePaths(changeFilePath, actions) {\n    let changeList = readFromPath(changeFilePath);\n    let filePaths = new Set();\n    if(actions?.length) {\n        changeList = changeList.filter(change => actions.includes(change.a.toLowerCase()));\n    };\n    changeList.forEach(change => {\n        if(change.j) {\n            const filesToBeAdded = (JSON.parse(change.j).filePath).filter(file => existsSync(file) && statSync(file).isFile());\n            filePaths = new Set([...filePaths, ...filesToBeAdded]);\n          }\n    });\n    return [...filePaths];\n}",
                    "copado__Timeout__c": 120,
                    "copado__Type__c": "Standard",
                    "copado__Worker_Size__c": "M",
                    "Id": "a0l7Q000000MDYvQAO",
                    "LastReferencedDate": "2022-07-29T07:34:19.000+0000",
                    "LastViewedDate": "2022-07-29T07:34:19.000+0000",
                    "Name": "Deploy"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0l7Q000000MDYwQAO"
                    },
                    "copado__API_Name__c": "sfdx_commit",
                    "copado__Callback_Type__c": "Flow",
                    "copado__Description__c": "Performs git commit on user stories",
                    "copado__FlowHandler__c": "cmcSf.Commit_Function_Callback",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"file_changes_id\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.fileWithSelectedChanges}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"file_name\",\n  \"defaultValue\" : \"Copado Commit changes.json\"\n}, {\n  \"required\" : true,\n  \"name\" : \"source_sessionid\",\n  \"defaultValue\" : \"{$Source.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"source_endpoint\",\n  \"defaultValue\" : \"{$Source.Credential.Endpoint}\"\n}, {\n  \"name\" : \"namespace\",\n  \"defaultValue\" : \"\"\n}, {\n  \"name\" : \"source_path\",\n  \"defaultValue\" : \"force-app\"\n}, {\n  \"required\" : true,\n  \"name\" : \"git_json\",\n  \"defaultValue\" : \"{$Context.Repository.Credential}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"base_branch\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.baseBranch}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"source_env\",\n  \"defaultValue\" : \"{$Source.apex.EnvironmentVariables}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"find_and_replace_rules\",\n  \"defaultValue\" : \"{$Context.apex.GlobalFindAndReplaceSourceId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"feature_branch\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.featureBranchName}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"recreate_if_exists\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.recreateFeatureBranch}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"commit_message\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.message}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"git_email\",\n  \"defaultValue\" : \"{$User.Email}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"git_name\",\n  \"defaultValue\" : \"{$User.Name}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"source_environment_branch\",\n  \"defaultValue\" : \"{$Context.apex.SourceEnvironmentBranch}\"\n}, {\n  \"name\" : \"git_depth\",\n  \"defaultValue\" : \"100\"\n}, {\n  \"name\" : \"timeout\",\n  \"defaultValue\" : \"180000\"\n}, {\n  \"name\" : \"chunkSize\",\n  \"defaultValue\" : \"10\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n/**\n * Performs commit of selected user story metadata changes.\n * Returns (If ACTION success) new feature branch in user repo with all the changes and returns the commit id in the result record\n * (If ACTION failed) Returns details with error status on user story commit record\n * @param file_changes_id\n * @param file_name\n * @param source_sessionid\n * @param source_endpoint\n * @param namespace\n * @param source_path\n * @param git_json\n * @param base_branch\n * @param source_env\n * @param find_and_replace_rules\n * @param feature_branch\n * @param recreate_if_exists\n * @param commit_message\n * @param git_email\n * @param git_name\n * @param source_environment_branch\n*/\nconst { execSync, exec } = require('child_process'),\n    { writeFileSync, readFileSync, existsSync, rmSync, statSync } = require('fs'),\n    { env, chdir, exit } = require('process'),\n    sourceApiVersion = \"55.0\",\n    targetDirectory = \"/app/repository\",\n    { feature_branch, source_sessionid } = env,\n    commitChangesFilePath = \"/app/encoded_changes.json\",\n    retrieveResultPath = \"/tmp/retrieveResult.json\",\n    ACTIONS = {\n        ADD: 'add',\n        RETRIEVE_ONLY: 'retrieveonly',\n        FULL: 'full',\n        DELETE: 'delete'\n    },\n    TYPES = {\n        PERMISSION_SET: 'permissionset',\n        PROFILE: 'profile'\n    },\n    STDIO = {\n        INHERIT: 'inherit',\n        PIPE: 'pipe',\n        IGNORE: 'ignore'\n    }\n\nlet commitChanges = {};\n\n// EXECUTION\n\nexecute();\n\nfunction execute() {\n    try {\n        encodeFileNames();\n        fetchCreateBranch();\n        prepareMetadataChangesList();\n    \n        if(commitChanges.addOrRetrieve.length > 0 || commitChanges.profiles.length > 0) { \n          Promise.all(retrieveOrgMetadata()).then(() => {\n          enrichChangeList(commitChangesFilePath, true);\n          varReplace();\n          findAndReplace();\n          processMetadata();\n          discardRetrieveOnlyFiles();\n          commitAndUpdateEnvironmentBranches();\n          }).catch((err) => {\n                console.log(err.toString());\n                executeCommand(`copado -p \"Error\" -e \"${err.toString()}\"`, STDIO.IGNORE);\n                exit(1);\n          });\n        } else {\n          enrichChangeList(commitChangesFilePath, false);\n          processMetadata();\n          commitAndUpdateEnvironmentBranches();\n        }\n    } catch(err) {\n        console.log(err.toString());\n        executeCommand(`copado -p \"Error\" -e \"${err.toString()}\"`, STDIO.IGNORE);\n        exit(1);\n    }\n}\n\n// SCRIPT FUNCTIONS\n\nfunction encodeFileNames() {\n    const { file_name, file_changes_id } = env;\n    const downloadCommitChangesJson = `copado --downloadfiles ${file_changes_id} --downloaddir /tmp/`;\n\texecuteCommand(downloadCommitChangesJson);\n    committedMetadata = JSON.parse(readFileSync(`/tmp/${file_name}`, 'utf-8'));\n\n    for (let commitChange of committedMetadata) { \n        switch (commitChange.t) {\n            case 'DashboardFolder':\n            case 'ReportFolder':\n            case 'Document':\n                commitChange.n = commitChange.n.replace(/%2F/gi, '/');\n                break;\n\n            case 'EmailTemplate':\n                commitChange.n = commitChange.n.replace(/%24/gi, '$').replace(/%2F/gi, '/');\n                break;\n \n            case 'Layout':\n                const regExp = '(?:([a-zA-Z_][a-zA-Z0-9_]{0,14}?(?!__c-))__)?([^-]+)-(?:([a-zA-Z_][a-zA-Z0-9_]{0,14}?_?(?!__c$))__)?(.+)\\n?';\n                /*\n                    The above regExp divides the layout full name into 4 groups:\n                    1. customobject namespace\n                    2. customobject name\n                    3. layout namespace\n                    4. layout mame\n                    The match function returns these groups respectively in the indices 1,2,3,4\n                */\n               const layoutFullName = commitChange.n.match(regExp);\n                let layoutName = layoutFullName[4].replace(/_{2}(?!c)/g, (match) => {return match.replace(/_/gi,'%5F') });\n                commitChange.n = `${layoutFullName[1] ? layoutFullName[1] + \"__\" : \"\"}${layoutFullName[2]}-${layoutFullName[3] ? layoutFullName[3] + \"__\" : \"\"}${layoutName}`;\n                break;\n        }\n\t}\n    writeFileSync(commitChangesFilePath, JSON.stringify(committedMetadata));\n    return committedMetadata;\n}\n\nfunction prepareMetadataChangesList() {\n    commitChanges.all = require(commitChangesFilePath);\n    commitChanges = { ...commitChanges,\n      addOrRetrieve: commitChanges.all.filter(selectedMetadata => (selectedMetadata.a.toLowerCase() == ACTIONS.RETRIEVE_ONLY || selectedMetadata.a.toLowerCase() == ACTIONS.ADD || (selectedMetadata.a.toLowerCase() == ACTIONS.FULL && selectedMetadata.t.toLowerCase() == TYPES.PERMISSION_SET))),\n      delete: commitChanges.all.filter(selectedMetadata => selectedMetadata.a.toLowerCase() == ACTIONS.DELETE),\n      profiles: commitChanges.all.filter(selectedMetadata => selectedMetadata.a.toLowerCase() == ACTIONS.FULL && selectedMetadata.t.toLowerCase() == TYPES.PROFILE),\n      retrieveOnly: commitChanges.all.filter(selectedMetadata => selectedMetadata.a.toLowerCase() == ACTIONS.RETRIEVE_ONLY),\n      getMetadataList(type) {     \n        return this[type].map(step => step.t + ':' + step.n).toString(',');\n      }\n\t};\n}\n\n\nfunction setup() {\n    configureGit();\n    checkSFDXProjectJson(feature_branch);\n    configureSFDXCLI();\n}\n\nfunction checkSFDXProjectJson(branchName) {\n    const sfdxProjectJsonPath = `${targetDirectory}/sfdx-project.json`;\n    if(existsSync(sfdxProjectJsonPath)) {\n        let fileContent = JSON.parse(readFileSync(sfdxProjectJsonPath, { encoding:'utf8' }));\n        if(fileContent.sourceApiVersion !== sourceApiVersion) {\n            const commitMessage = `Updated  sourceApiVersion from ${fileContent.sourceApiVersion} to ${sourceApiVersion} in sfdx-project.json to align the commit, promote and deploy operations with the latest supported api version of Copado`;\n            fileContent.sourceApiVersion = sourceApiVersion;\n            writeFileSync(sfdxProjectJsonPath, JSON.stringify(fileContent, null, 2));\n            gitCommit(commitMessage, branchName); \n        }\n    } else {\n        throw `Invalid configuration in ${branchName}. sfdx-project.json is invalid or missing at project root. Copado Commit and Deploy operations are required to run from within a valid sfdx project.`;\n    }\n}\n\nfunction configureSFDXCLI() {\n    const { source_endpoint } = env;\n    const baseUrl = source_endpoint.substring(0, source_endpoint.indexOf('/', source_endpoint.indexOf('/') + 2));\n\n    const configSet = `\n    sfdx config:set instanceUrl=${baseUrl}\n    sfdx config:set apiVersion=${sourceApiVersion}`;\n    executeCommand(configSet);\n}\n\nfunction sfdxRetrieve(resolve, reject) {\n\n    const retrieve = `\n        sfdx force:source:retrieve --json -u \"${source_sessionid}\" -m '${commitChanges.getMetadataList('addOrRetrieve')}' || true\n    `;\n    exec(retrieve, (err, retrieveResult, stderr) => {\n        // Adding the retrive result to the logs\n        console.log(retrieveResult);\n        retrieveResult = JSON.parse(retrieveResult.toString());\n\n        if(retrieveResult.status  && retrieveResult.message) {\n           reject(retrieveResult.message);\n        }\n        \n        const failedMetadataList = retrieveResult?.result?.inboundFiles?.filter(file => file.state == 'Failed');\n        if(failedMetadataList?.length) \n        {\t\n            const err = failedMetadataList.map(metadata => metadata.error).toString();\n            reject(err);\n        }\n        writeFileSync(retrieveResultPath, JSON.stringify(retrieveResult));\n        resolve(retrieveResult);\n    });\n}\n\nfunction retrieveFullProfile(resolve, reject) {\n\t\n    \tconst retrieveFullProfile = `\n        sfdx sfpowerkit:source:profile:retrieve -u \"${source_sessionid}\" -n \"${commitChanges.profiles.map(profile => profile.n).toString()}\" \n        `\n        exec(retrieveFullProfile, { stdio: 'inherit' }, (err, profileResult, stderr) => {\n    \t\tif(err) {\n                console.log(profileResult);\n            \treject(\"Full Profile Retrieval Failed\");\n            } else {\n                rmSync(\"sfpowerkit-cache.db\", {\n                    force: true,\n                });\n        \t\tresolve();\n            }\n        });\n}\nfunction retrieveOrgMetadata() {\n\n    setup();\n    const setProgress = `\n    copado -p \"Retrieving the selected metadata\"`\n    executeCommand(setProgress);\n    const promises = [];\n    commitChanges.profiles.length && promises.push( new Promise((resolve, reject) => {\n        retrieveFullProfile(resolve, reject);\n    }));\n    commitChanges.addOrRetrieve.length && promises.push(new Promise((resolve, reject) => {\n        sfdxRetrieve(resolve, reject);\n    }));\n    return promises;\n}\n\nfunction varReplace() {\n    const filesToIncludeListPath = '/tmp/filesToInclude.json',\n    filesToInclude = getFilePaths(commitChangesFilePath, [ ACTIONS.ADD, ACTIONS.FULL ] );\n    if(filesToInclude?.length) {\n        writeFileSync(filesToIncludeListPath, JSON.stringify(filesToInclude));\n        const { source_env } = env,\n            varreplace = `\n            copado -p \"Replacing environment variables, if any\"\n            varreplace '${source_env}' '${targetDirectory}' --valuename=true  --include=${filesToIncludeListPath} || ${getErrorCmdString(\"Error replacing environment variables\")}`;\n            executeCommand(varreplace, STDIO.INHERIT);\n    }\n}\n\nfunction fetchCreateBranch() {\n    const { recreate_if_exists, base_branch, git_depth } = env,\n        fetchCreateBranch = `\n    mkdir -p ${targetDirectory} && cd ${targetDirectory}\n\n    copado-git-get --depth \"${git_depth}\" \"${base_branch}\" || ${getErrorCmdString(\"Error fetching \" + base_branch)}\n    if [ -n \"${feature_branch}\" ]; then\n      if [ \"${recreate_if_exists}\" == \"true\" ]; then\n        copado -p \"Removing branch ${feature_branch}\"\n        git branch -D \"${feature_branch}\" || true\n        git push origin --delete \"${feature_branch}\" || true\n      fi\n        copado -p \"Fetching/creating branch ${feature_branch}\"\n        copado-git-get -c \"${feature_branch}\" ||  ${getErrorCmdString(\"Error fetching/creating \" + feature_branch)}\n    fi\n    `\n    executeCommand(fetchCreateBranch);\n    chdir(`${targetDirectory}`);\n}\n\nfunction findAndReplace() {\n    const { find_and_replace_rules } = env,\n        pathToYaml = \"/tmp/Copado\";\n    if(find_and_replace_rules) {\n        const findAndReplace = `\n        copado -p \"Applying global find and replace rules, if any\"\n        copado --downloadfiles \"${find_and_replace_rules}\" --downloaddir /tmp/\n        yamlreplace \"${pathToYaml}\" \"${targetDirectory}\" -b \"${feature_branch}\" ||  ${getErrorCmdString(\"Error applying find and replace rules\")}` \n        executeCommand(findAndReplace, STDIO.INHERIT);\n    }\n}\n\nfunction enrichChangeList(filePath, includeCliResponse) {\n    executeCommand(`enricher -p ${filePath} --repo ${targetDirectory}/ ${ includeCliResponse ? ( existsSync(retrieveResultPath) ? '--cliresponse ' + retrieveResultPath : '') : ' '} || ${getErrorCmdString(\"Error processing metadata\")}`, STDIO.INHERIT);\n}\n\nfunction processMetadata() {\n    const pocesssMetadata = `\n            copado -p \"Processing metadata\"\n            metadata-processor \"${commitChangesFilePath}\" \"${targetDirectory}\" -o COMMIT ||  ${getErrorCmdString(\"Error processing metadata\")}\n        `;\n        executeCommand(pocesssMetadata);\n}\n\nfunction gitCommit(commitMessage, branchName) {\n    const  gitCommit = `\n    copado -p \"Committing ${commitMessage} in ${branchName}\"\n    git add .\n    git status\n    git commit -m \"${commitMessage}\" || ${getErrorCmdString(\"There was some issue when committing metadata\")}\n    `;\n    executeCommand(gitCommit);\n}\n\nfunction configureGit () {\n    const { git_email,  git_name } = env,\n    configureGit = `\n        git config --local user.email \"${git_email}\" || exit 1\n        git config --local user.name \"${git_name}\" || exit 1\n        git config --local diff.renames false || exit 1\n        git config --local merge.renames false || exit 1\n        git config --local status.renames false || exit 1\n        `;\n    executeCommand(`${configureGit}`, STDIO.IGNORE);\n}\n\nfunction commitAndUpdateEnvironmentBranches() {\n    const { commit_message, source_environment_branch} = env,\n    gitStatus = executeCommand(`\n    git status --porcelain`);\n    if(!gitStatus) {\n        executeCommand(`copado -p \"There are no changes to be committed\" -r '{ \"status\" : \"No Changes\" }' && exit 0`);\n    }\n    else {\n        const saveCommitId = `\n        commit_id=$(git rev-parse HEAD)\n        resultdata='{ \"commitId\": \"'$commit_id'\" }'\n        copado -p \"Saving commit id\" --result-data \"$resultdata\"  || ${getErrorCmdString('Error saving commit id')}\n        `;\n\n        const fetchSourceBranch = `\n        copado -p \"Fetching source branch - ${source_environment_branch}\"\n        copado-git-get \"${source_environment_branch}\"\n        `;\n\n        const mergeFeatureBranchInSource = `\n        copado -p \"Merging changes to source branch - ${source_environment_branch}\"\n\n        # We have used || true with git merge because:\n        # 1. We do not want this command to fail. After merge, if there is a conflict, we only want to find the git status after the merge and send it to the copado-merge service and they will handle the conflicts for us.\n        # 2. If we fail this command on a conflict, then we will not be able to send control to the copado-merge service.\n        git merge \"${feature_branch}\" -Xignore-space-change || true\n        copado -p \"Resolving git conflicts, if any\"\n        git status --porcelain=v1\n        git status --porcelain=v1 > /tmp/git_status.txt\n        echo \"git_status\"\n        cat /tmp/git_status.txt\n        touch /tmp/output.json\n        copado-merge ${targetDirectory} ${commitChangesFilePath} -p /tmp/git_status.txt -t sfdx -c --out /tmp ||  ${getErrorCmdString(\"There was some issue merging \" + feature_branch + \" into \" + source_environment_branch)}\n        git add . # add all the resolved changes, if any\n        git commit -am \"Merging ${feature_branch} into ${source_environment_branch} after auto conflict resolution\" || true\n        `;\n        \n        gitCommit(commit_message, feature_branch);\n        gitPush(feature_branch);\n        \n        executeCommand(`\n        ${saveCommitId}\n        ${fetchSourceBranch}`, STDIO.INHERIT);\n\n        executeCommand(mergeFeatureBranchInSource);\n        checkSFDXProjectJson(source_environment_branch);\n        gitPush(source_environment_branch);\n    }\n}\n\nfunction gitPush(branchName) {\n    const gitPush = `\n    copado -p \"Pushing all changes to ${branchName}\"\n    git push origin \"${branchName}\" --atomic ||  ${getErrorCmdString(\"Could not push the changes\")}\n    `;\n    executeCommand(gitPush);\n}\n\nfunction getErrorCmdString(error) {\n    const suffix = 'Please check the logs for details.'\n    return `{ copado -p \"Error\" -e \"${error}. ${suffix}\"; exit 1; }`\n}\n\nfunction readFromPath(filePath) {\n    return JSON.parse(readFileSync(filePath, 'utf-8'));\n}\n\nfunction getFilePaths(changeFilePath, actions) {\n    let changeList = readFromPath(changeFilePath);\n    let filePaths = new Set();\n    if(actions?.length) {\n        changeList = changeList.filter(change => actions.includes(change.a.toLowerCase()));\n    };\n    changeList.forEach(change => {\n        if(change.j && change.j !== '') {\n            const filesToBeAdded = (JSON.parse(change.j)?.filePath)?.filter(file => existsSync(file) && statSync(file).isFile());\n            if(filesToBeAdded) {\n                filePaths = new Set([...filePaths, ...filesToBeAdded]);\n            }\n          }\n    });\n    return [...filePaths];\n}\n\nfunction discardRetrieveOnlyFiles() {\n    if(commitChanges.retrieveOnly?.length) {\n        const retrieveChangesPath = '/tmp/retrieveOnlyFiles.json';\n        writeFileSync(retrieveChangesPath, JSON.stringify(commitChanges.retrieveOnly));\n        enrichChangeList(retrieveChangesPath, false);\n        discardFilesInGit(getFilePaths(retrieveChangesPath));\n    }\n}\n\nfunction getUntrackedFiles() {\n    return executeCommand(`git ls-files --others --exclude-standard`).split('\\n');\n}\n\nfunction executeCommandinChunks(fileList, cmd) {\n    const { chunkSize } = env;\n    if(fileList.length > 0) {\n        let fileIndex = 0;\n        do {\n            let endIndex = fileIndex + parseInt(chunkSize) > fileList.length ? fileList.length : fileIndex + parseInt(chunkSize);\n            let fileChunk = fileList.slice(fileIndex, endIndex);\n            executeCommand(cmd.replace(/REPLACE_VALUE/g, fileChunk.map(file => `'${file}'`).join(' ')));\n            fileIndex =  endIndex;\n        } while(fileIndex < fileList.length);\n    }\n}\n\nfunction discardFilesInGit(fileList) {\n    const addedFiles = getFilePaths(commitChangesFilePath, [ ACTIONS.ADD ]);\n    if(addedFiles?.length) {\n        fileList = fileList.filter(file =>  !(addedFiles.includes(file)));\n    }\n    const unTrackedFiles = getUntrackedFiles();\n    const toBeCleanedFiles = fileList.filter(file => unTrackedFiles.includes(file));\n    const trackedFiles = fileList.filter(file => !(toBeCleanedFiles.includes(file)));\n    executeCommandinChunks(toBeCleanedFiles, `git clean -fd REPLACE_VALUE`);\n    executeCommandinChunks(trackedFiles, `( git checkout HEAD -- REPLACE_VALUE ) || true`);\n}\n\nfunction executeCommand(cmd, ioconfig) {\n    const { timeout } = env;\n    const options = {};\n    if(ioconfig) {\n        options.stdio = ioconfig;\n    }\n    options.timeout = parseInt(timeout);\n    return execSync(cmd, options)?.toString();\n}",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1",
                    "copado__Worker_Size__c": "M",
                    "Id": "a0l7Q000000MDYwQAO",
                    "LastReferencedDate": "2022-08-02T09:08:48.000+0000",
                    "LastViewedDate": "2022-08-02T09:08:48.000+0000",
                    "Name": "Commit"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0l7Q000000MDYxQAO"
                    },
                    "copado__API_Name__c": "sfdx_promote",
                    "copado__Callback_Type__c": "Flow",
                    "copado__Description__c": "Creation of Promotion Branch and Promotion of user stories",
                    "copado__FlowHandler__c": "cmcSf.Update_Conflict_Resolution_File",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"user_stories\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.userStoryBranches}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"promotion_branch\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.promotionBranchName}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"target_branch\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.destinationBranchName}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"tag\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.Promotion__r.Release__r.Version__c}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"git_json\",\n  \"defaultValue\" : \"{$Context.Repository.Credential}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"recreatePromotionBranch\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.Promotion__r.cmcSf__Recreate_Promotion_Branch__c}\"\n}, {\n  \"name\" : \"promotion_id\",\n  \"defaultValue\" : \"{$Job.ExecutionParent.Id}\"\n}, {\n  \"name\" : \"file_changes_id\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.fileWithSelectedChanges}\"\n}, {\n  \"name\" : \"conflict_resolution_attachments\",\n  \"defaultValue\" : \"{$Context.apex.GetConflictResolutionAttachments}\"\n}, {\n  \"name\" : \"git_name\",\n  \"defaultValue\" : \"{$User.Name}\"\n}, {\n  \"name\" : \"git_email\",\n  \"defaultValue\" : \"{$User.Email}\"\n}, {\n  \"name\" : \"repository_id\",\n  \"defaultValue\" : \"{$Pipeline.Git_Repository__r.Id}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"git_depth\",\n  \"defaultValue\" : \"100\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n/**\n * Performs promotion of selected user story metadata changes.\n * Returns (If ACTION success) new peomotion branch in user repo with all the changes\n * (If ACTION failed) Returns details with conflict or error status on promotion record\n * @param user_stories\n * @param promotion_branch\n * @param target_branch\n * @param tag\n * @param recreate_if_exists\n * @param promotion_id\n * @param file_changes_id\n * @param conflict_resolution_attachments\n * @param git_name\n * @param git_email\n * @param repository_id\n * @param git_depth\n*/\n\nconst { execSync } = require('child_process'),\n    { existsSync, readFileSync, writeFileSync } = require('fs'),\n    process = require('process'),\n    targetDirectory = '/app/repository',\n    sourceApiVersion = \"55.0\";\n\nconst {\n    file_changes_id,\n    target_branch,\n    promotion_branch,\n    git_depth,\n    git_email,\n    git_name,\n    promotion_id,\n    repository_id,\n    conflict_resolution_attachments,\n    user_stories,\n    tag,\n    recreatePromotionBranch\n} = process.env;\n\n// EXECUTION\n\nexecute();\n\nfunction execute() {\n\n    try {\n        fetchCreatePromotionBranch();\n        checkSFDXProjectJson();\n        configureGit();\n        promote();\n        gitPush();\n    } catch (err) {\n        execSync(`copado -p 'Error' -e \"${err.toString()}\" && exit 1`);\n    }\n}\n\n// SCRIPTS\n\nfunction fetchCreatePromotionBranch() {\n\n    let dirSetupCommand = `\n        rm -rf ${targetDirectory}\n        mkdir ${targetDirectory}\n    `\n    execSync(dirSetupCommand, {\n        stdio: 'ignore'\n    });\n    process.chdir(targetDirectory);\n\n    let command = `\n        copado --downloadfiles \"${file_changes_id}\" --downloaddir /tmp/\n        copado-git-get --depth \"${git_depth ? git_depth : 100}\" \"${target_branch}\" || ${getErrorStringAndExit(\"Error fetching branch \" + target_branch)}\n        if [ -n \"${promotion_branch}\" ]; then\n         \techo recreate_if_exists: ${recreatePromotionBranch}\n            if [ ${recreatePromotionBranch} == \"true\" ]; then\n            exists=$(git ls-remote --heads origin ${promotion_branch} | wc -l)\n                if [ -n \"$exists\" ]; then\n                    copado -p \"Removing branch ${promotion_branch}\"\n                    git push origin --delete \"${promotion_branch}\" || true\n                fi\n            fi\n            copado -p \"Fetching/creating branch ${promotion_branch}\"\n            copado-git-get -c \"${promotion_branch}\" ||  ${getErrorStringAndExit(\"Error fetching/creating branch\" + promotion_branch)}\n        fi\n    `\n\n    execSync(command, {\n        stdio: 'inherit'\n    });\n}\nfunction configureGit(){\n    execSync(`\n        git config --local user.email \"${git_email}\" || exit 1\n        git config --local user.name \"${git_name}\" || exit 1\n        git config --local diff.renames false || exit 1\n        git config --local merge.renames false || exit 1\n        git config --local status.renames false || exit 1\n    `, {\n        stdio: 'inherit'\n    });\n}\n\nfunction promote() {\n    const outputJSONPath = '/tmp/output.json';\n\n    for(let story of JSON.parse(user_stories)) {\n        if(story.indexOf('/') === -1) {\n            throw `User Story branch - ${story} does not follow the Copado Branching strategy`;\n         }\n        let user_story_name = story.split('/')[1];\n\n        execSync(`\n            copado -p \"Merging ${story} into ${promotion_branch}\"\n            copado-git-get --depth \"${git_depth ? git_depth : 100}\" \"${story}\" ||  ${getErrorStringAndExit(\"Error fetching branch \" + story)}\n            enricher -p \"/tmp/Copado Promotion changes.json\" --repo ${targetDirectory}/ || ${getErrorStringAndExit(\"Error processing files\")}\n            git checkout \"${promotion_branch}\"\n            git merge -m \"Merging ${story} to ${promotion_branch}\" \"${story}\" -Xignore-space-change || true\n            copado -p \"Resolving git conflicts, if any\"\n            git status --porcelain=v1 > /tmp/git_status.txt\n       \t\techo \"${JSON.stringify(conflict_resolution_attachments)}\" > /tmp/solvedByUser.json\n\t\t\ttouch ${outputJSONPath}\n\n\t\t\tcopado-merge ${targetDirectory} '/tmp/Copado Promotion changes.json' -p /tmp/git_status.txt -r /tmp/solvedByUser.json -u ${user_story_name} -t sfdx -cp ${promotion_id} -rp ${repository_id} --out /tmp ||  ${getErrorStringAndExit(\"Error processing merge\")}\n            `, { stdio: 'inherit' });\n\n        let output;\n        if(existsSync(outputJSONPath)) {\n            output = JSON.parse(readFileSync(outputJSONPath, 'utf8'));\n            if(output?.some(file =>  file.strategy === \"ONLINE_CONFLICT_RESOLUTION\")) {\n                execSync(`copado -p \"Conflict found while merging ${story} to $promotion_branch. Conflict needs to be resolved manually.\" -r '{\"status\": \"conflicts\"}'\n                    exit 1`, { stdio: 'inherit' });\n            }\n        }\n        gitCommit(`Merging ${story} to ${promotion_branch} after auto conflict resolution`);\n        saveMergeCommitId(output);\n    }\n}\n \nfunction gitCommit(commitMessage) {\n    execSync(`           \n        git add . # add all the resolved changes, if any \n        git commit -am \"${commitMessage}\" || true`, {\n        stdio: 'inherit'\n    });\n}\n\nfunction saveMergeCommitId(output) {\n    if(output?.length) {\n        const conflictResolutionsFilePath = '/tmp/GitConflictsResolution.json';\n        const mergeCommitId = execSync(`git rev-parse HEAD`).toString().trim();\n        output?.map(file => file.sucessfulPromotion = mergeCommitId);\n        writeFileSync(conflictResolutionsFilePath, JSON.stringify(output, null, 2));\n        execSync(`copado --uploadfile ${conflictResolutionsFilePath}`, {\n            stdio: 'inherit'\n        });\n    }\n}\n\nfunction gitPush(){\n    execSync(`\n        copado -p \"Pushing all changes\"        \n        if [ -n \"${tag}\" ]; then\n            git tag \"${tag}\"\n            git push --all --atomic || ${getErrorStringAndExit(\"Could not push the changes\")}\n        else\n            echo \"No tag specified\"\n            git push origin \"${promotion_branch}\" --atomic || ${getErrorStringAndExit(\"Could not push the changes\")}\n        fi\n    `, {\n        stdio: 'inherit'\n    });\n}\n\nfunction checkSFDXProjectJson() {\n    const sfdxProjectJsonPath = `${targetDirectory}/sfdx-project.json`;\n    if(existsSync(sfdxProjectJsonPath)) {\n        let fileContent = JSON.parse(readFileSync(sfdxProjectJsonPath, { encoding:'utf8' }));\n        if(fileContent.sourceApiVersion !== sourceApiVersion) {\n            const commitMessage = `Updated  sourceApiVersion from ${fileContent.sourceApiVersion} to ${sourceApiVersion} in sfdx-project.json to align the commit, promote and deploy operations with the latest supported api version of Copado.`;\n            fileContent.sourceApiVersion = sourceApiVersion;\n            writeFileSync(sfdxProjectJsonPath, JSON.stringify(fileContent, null, 2));\n            gitCommit(commitMessage);\n        }\n    } else {\n        throw \"Invalid configuration. sfdx-project.json is invalid or missing at project root. Copado Commit and Deploy operations are required to run from within a valid sfdx project.\";\n    }\n}\n\nfunction getErrorStringAndExit(err) {\n    const suffix = \"Please check the logs for details.\"\n    return  `{ copado -p \"Error\" -e \"${err}. ${suffix}\"; exit 1; }`;\n}",
                    "copado__Type__c": "Standard",
                    "copado__Worker_Size__c": "M",
                    "Id": "a0l7Q000000MDYxQAO",
                    "LastReferencedDate": "2022-07-28T12:32:13.000+0000",
                    "LastViewedDate": "2022-07-28T12:32:13.000+0000",
                    "Name": "Promote"
                },
                {
                    "attributes": {
                        "type": "copado__Function__c",
                        "url": "/services/data/v54.0/sobjects/copado__Function__c/a1t5f00000121TGAAY"
                    },
                    "copado__API_Name__c": "sfdx_encode_file_names",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"name\" : \"file_changes_id\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.fileWithSelectedChanges}\"\n}, {\n  \"name\" : \"file_name\",\n  \"defaultValue\" : \"Copado Deploy changes.json\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\nconst execSync = require('child_process').execSync,\n\t{ file_name, file_changes_id } = process.env,\n    fs = require('fs');\n    \n\tconst downloadCommitChangesJson = `copado --downloadfiles ${file_changes_id} --downloaddir /tmp/`;\n\texecSync(downloadCommitChangesJson);\n\n    const committedMetadata = require(`/tmp/${file_name}`);\n\n    console.log(committedMetadata);\n    for (let commitChange of committedMetadata) { \n        switch (commitChange.t) {\n            case 'DashboardFolder':\n            case 'ReportFolder':\n            case 'Document':\n                commitChange.n = commitChange.n.replace(/%2F/gi, '/');\n                break;\n\n            case 'EmailTemplate':\n                commitChange.n = commitChange.n.replace(/%24/gi, '$').replace(/%2F/gi, '/');\n                break;\n \n            case 'Layout':\n                const regExp = '(?:([a-zA-Z_][a-zA-Z0-9_]{0,14}?(?!__c-))__)?([^-]+)-(?:([a-zA-Z_][a-zA-Z0-9_]{0,14}?_?(?!__c$))__)?(.+)\\n?';\n                const layoutFullName = commitChange.n.match(regExp);\n                let layoutName = layoutFullName[4].replace(/_{2}(?!c)/g, (match) => {return match.replace(/_/gi,'%5F') });\n                commitChange.n = `${layoutFullName[1] ? layoutFullName[1] + \"__\" : \"\"}${layoutFullName[2]}-${layoutFullName[3] ? layoutFullName[3] + \"__\" : \"\"}${layoutName}`;\n                break;\n        }\n\t}\nfs.writeFileSync('/app/encoded_changes.json', JSON.stringify(committedMetadata));",
                    "copado__Type__c": "Standard",
                    "copado__Worker_Size__c": "S",
                    "Id": "a1t5f00000121TGAAY",
                    "LastReferencedDate": "2022-04-12T13:08:00.000+0000",
                    "LastViewedDate": "2022-04-12T13:08:00.000+0000",
                    "Name": "Encode File Names"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v54.0/sobjects/copado__Function__c/a0l7Q000000MDYzQAO"
                    },
                    "copado__API_Name__c": "sfdx_package_version_publish",
                    "copado__Description__c": "To publish the package version",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"packageVersionId\",\n  \"defaultValue\" : \"{$Job.PrevStep.Result__r.Result_Data__c}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"SESSION\",\n  \"defaultValue\" : \"{$Destination.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"BASEURL\",\n  \"defaultValue\" : \"{$Destination.Credential.Endpoint}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n\nconst { execSync } = require(\"child_process\"),\n    { packageVersionId, BASEURL, SESSION } = process.env,\n    checkLog = \"Please check the logs for details\";\n   \n// EXECUTION\ntry {\n    BASEURL && setInstanceURL();\n    createSFDXProject();\n    handleVersionPromotionResponse(promotePackageVersion());\n    \n    execSync(`copado -p 'Updating Results' -r ${packageVersionId}`);\n} catch (err) {\n    execSync(showError(err.toString()));\n}\n\n// SCRIPT FUNCTIONS\nfunction setInstanceURL() {\n    execSync(`\n        copado -p 'Setting instance url'\n        sfdx force:config:set instanceUrl=${BASEURL.substring(0, BASEURL.indexOf(\"/\", BASEURL.indexOf(\"/\") + 2))} --global || (${showError(`Error setting instance URL, ${checkLog}`)})\n    `);\n}\nfunction showError(error) {\n    return `copado -p 'Error' -e \"${error}\" && exit 1`;\n  }\nfunction createSFDXProject() {\n    execSync(`\n        copado -p 'Creating Temp Project'\n        sfdx force:project:create -n temp || (${showError(`Error creating SFDX project, ${checkLog}`)})\n    `);\n}\nfunction promotePackageVersion() {\n    const promotePackage = ` \n        copado -p 'Promoting package ${packageVersionId}'\n        cd temp\n        sfdx force:package:version:promote -p ${packageVersionId} -v ${SESSION} -n --json || true\n        copado -p 'Package '${packageVersionId}' promoted correctly'\n    `;\n    return JSON.parse(execSync(promotePackage).toString());\n}\n\nfunction handleVersionPromotionResponse(promotionResponse) {\n    promotionResponse.status === 1 && execSync(showError(`${promotionResponse.message}, ${checkLog}`));\n}",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1.0",
                    "copado__Worker_Size__c": "S",
                    "Id": "a0l7Q000000MDYzQAO",
                    "LastReferencedDate": "2022-05-10T06:56:31.000+0000",
                    "LastViewedDate": "2022-05-10T06:56:31.000+0000",
                    "Name": "Sfdx Package Version Publish"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0l7Q000000iAiPQAU"
                    },
                    "copado__API_Name__c": "SFDX_Package_Version_Update",
                    "copado__Description__c": "Updates package version",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"SESSION\",\n  \"defaultValue\" : \"{$Context.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"ENDPOINT\",\n  \"defaultValue\" : \"{$Context.Credential.Endpoint}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"packageVersion\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.packageVersion}\"\n}, {\n  \"name\" : \"installationKey\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.installationKey}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\nconst { execSync } = require(\"child_process\"),\n\t{ ENDPOINT, SESSION } = process.env,\n\tbaseUrl = ENDPOINT.substring(\n\t\t0,\n\t\tENDPOINT.indexOf(\"/\", ENDPOINT.indexOf(\"/\") + 2)\n\t),\n\tcheckLog = \"Please check the logs for details\",\n\tpackageVersion = JSON.parse(process.env.packageVersion),\n\tsubscriberVersionId = packageVersion.copado__Subscriber_Version_Id__c,\n\tversionName = packageVersion.copado__Version_Name__c,\n\tdescription = packageVersion.copado__Version_Description__c,\n\tbranch = packageVersion.copado__Branch__c,\n\ttag = packageVersion.copado__Tag__c,\n\tinstallationKey = process.env.installationKey;\n\n// EXECUTION\n\ntry {\n\tsetInstanceURL();\n\tcreateSFDXProject();\n\thandleVersionPromotionResponse(updatePackageVersion());\n\n\texecSync(\n\t\t`copado -p 'Updating result' -r '${JSON.stringify({\n\t\t\tpackageVersion,\n\t\t\tinstallationKey,\n\t\t})}'`\n\t);\n} catch (err) {\n\texecSync(showError(err.toString()));\n}\n\n// SCRIPT FUNCTIONS\n\nfunction buildParameters(flags) {\n\treturn Object.entries(flags).reduce((parameters, [key, value]) => {\n\t\treturn parameters + (value ? ` --${key} '${value}'` : \"\");\n\t}, \"\");\n}\n\nfunction setInstanceURL() {\n\texecSync(`\n        copado -p 'Setting instance url'\n        sfdx force:config:set instanceUrl=${baseUrl} --global || (${showError(\n\t\t`Error setting instance URL, ${checkLog}`\n\t)})\n    `);\n}\n\nfunction createSFDXProject() {\n\texecSync(`\n        copado -p 'Creating Temp Project'\n        sfdx force:project:create -n temp || (${showError(\n\t\t`Error creating SFDX project, ${checkLog}`\n\t)})\n    `);\n}\n\nfunction showError(error) {\n\treturn `copado -p 'Error' -e \"${error}\" && exit 1`;\n}\n\nfunction updatePackageVersion() {\n\tconst parameters = buildParameters({\n\t\ttargetdevhubusername: SESSION,\n\t\tpackage: subscriberVersionId,\n\t\tversionname: versionName,\n\t\tversiondescription: description,\n\t\tbranch: branch,\n\t\ttag: tag,\n\t\tinstallationkey: installationKey,\n\t});\n\tconst updateCmd = `\n        copado -p 'Updating package version'\n        cd temp\n        sfdx force:package:version:update ${parameters} --json || true\n    `;\n\treturn JSON.parse(execSync(updateCmd).toString());\n}\n\nfunction handleVersionPromotionResponse(promotionResponse) {\n\tif(promotionResponse.status === 1) {\n\t\texecSync(\n\t\t\tshowError(\n\t\t\t\t`Package Version Updated failed. ${promotionResponse.message}, ${checkLog}`\n\t\t\t)\n\t\t);\n\t}\n}",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1",
                    "copado__Worker_Size__c": "S",
                    "Id": "a0l7Q000000iAiPQAU",
                    "LastReferencedDate": "2022-06-28T11:21:45.000+0000",
                    "LastViewedDate": "2022-06-28T11:21:45.000+0000",
                    "Name": "SFDX Package Version Update"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0l7Q000000tRpUQAU"
                    },
                    "copado__API_Name__c": "SFDX_Package_Version_Create",
                    "copado__Description__c": "Create a package version record",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"ENDPOINT\",\n  \"defaultValue\" : \"{$Destination.Credential.Endpoint}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"SESSION\",\n  \"defaultValue\" : \"{$Destination.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"packageId\",\n  \"defaultValue\" : \"\"\n}, {\n  \"name\" : \"versionName\",\n  \"defaultValue\" : \"\"\n}, {\n  \"name\" : \"versionNumber\",\n  \"defaultValue\" : \"\"\n}, {\n  \"name\" : \"description\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : false,\n  \"name\" : \"jsonInformation\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : true,\n  \"name\" : \"git_json\",\n  \"defaultValue\" : \"{$Context.Repository.Credential}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n\nconst fs = require(\"fs\"),\n  execSync = require(\"child_process\").execSync,\n  endPoint = process?.env[\"ENDPOINT\"],\n  session = process?.env[\"SESSION\"],\n  packageId = process?.env[\"packageId\"],\n  params = JSON.parse(\n    process?.env[\"jsonInformation\"].replace(/(\\\\r\\\\n|\\\\r|\\\\n|\\\\)/g, \"\")\n  ),\n  branchName = params?.branch,\n  baseUrl = endPoint?.substring(\n    0,\n    endPoint?.indexOf(\"/\", endPoint?.indexOf(\"/\") + 2)\n  ),\n  tag = params?.tag,\n  checkLog = \"Please check the logs for details\",\n  projectDir = \"sfProject\";\n\n// EXECUTION\n\nexecute();\n\nfunction execute() {\n  try {\n    fs.mkdirSync(projectDir);\n    process.chdir(projectDir);\n\n    cloneRepo();\n    setInstanceURL();\n    updatePkgVersionCreation(JSON.parse(pkgVersionCreation()));\n  } catch (err) {\n    execSync(`${getErrorCmd(err.toString())}`);\n  }\n}\n\n// SCRIPT FUNCTIONS\nfunction updatePkgVersionCreation(response) {\n\n  if (!response.status) {\n    commit();\n    if (tag) {\n      const remoteTags = execSync(\n        `git ls-remote --tags --refs origin`\n      ).toString(),\n        isTagExist = remoteTags\n          .split(\"\\n\")\n          .map((step) => step.substring(step.lastIndexOf(\"/\") + 1, step.length))\n          .includes(tag);\n\n      createTag(tag, isTagExist);\n    }\n    pushTagAndBranchChanges();\n    setProgressStatus({\n      progressStatus: 'Package version created successfully', resultData: `${JSON.stringify(\n        getLatestVersion(response?.result?.SubscriberPackageVersionId)\n      )}`\n    })\n  } else if (response.status) {\n    setProgressStatus({ progressStatus: 'Package Version creation failed', errorMessage: `Could not create package. ${response.message}` })\n  }\n}\n\nfunction getLatestVersion(pkgVersionId) {\n  const versionList = execSync(\n    `sfdx force:package:version:list -v ${session} -p ${packageId} --json`\n  ).toString();\n  return JSON.parse(versionList)?.result?.find(\n    (version) => version.SubscriberPackageVersionId === pkgVersionId\n  );\n}\n\nfunction pkgVersionCreation() {\n  return execSync(`\n      copado -p 'Creating package version'\n      sfdx force:package:version:create ${getUpdateParams([\n        { key: 'loglevel', value: params?.loglevel, isString: false },\n        { key: 'targetdevhubusername', value: session, isString: false},\n        { key: 'apiversion', value: params?.apiversion,isString: false},\n        { key: 'package', value: packageId, isString: true},\n        { key: 'path', value: params?.path, isString: true},\n        { key: 'branch', value: branchName, isString: true},\n        { key: 'definitionfile', value: params?.definitionfile, isString: true},\n        { key: 'tag', value: tag, isString: true},\n        { key: 'installationkey', value: params?.installationkey, isString: true},\n        { key: 'installationkeybypass', value: params?.installationkeybypass, isString: false},\n        { key: 'wait', value: 1000, isString: false},\n        { key: 'versionname', value: process.env[\"versionName\"], isString: true},\n        { key: 'versionnumber', value: process?.env[\"versionNumber\"], isString: false},\n        { key: 'versiondescription', value: process?.env[\"description\"], isString: true},\n        { key: 'codecoverage', value: params?.codecoverage, isString: true},\n        { key: 'releasenotesurl', value: params?.releasenotesurl, isString: true},\n        { key: 'postinstallurl', value: params?.postinstallurl, isString: true},\n        { key: 'postinstallscript', value: params?.postinstallscript, isString: true},\n        { key: 'uninstallscript', value: params?.uninstallscript, isString: true},\n        { key: 'skipvalidation', value: params?.skipvalidation, isString: false}\n    ])} --json || true\n  `).toString();\n}\n\nfunction createTag(tag, tagExist) {\n  execSync(\n    `\n        ${tagExist ? `git push --delete origin refs/tags/${tag} || exit 1` : \"\"}\n    `,\n    { stdio: \"inherit\" }\n  );\n\n  execSync(\n    `\n        git tag ${tag} HEAD || exit 1\n    `,\n    { stdio: \"inherit\" }\n  );\n}\n\nfunction commit() {\n  execSync(\n    `\n        git add \"sfdx-project.json\" || exit 1\n        git commit -m \"Committing sfdx-project.json\" || exit 1\n    `,\n    { stdio: \"inherit\" }\n  );\n}\n\nfunction pushTagAndBranchChanges() {\n  execSync(\n    `git push origin ${branchName} || (${getErrorCmd(\n      `Error pushing changes in git branch, ${checkLog}`\n    )})`,\n    { stdio: \"inherit\" }\n  );\n\n  if(tag) {\n    execSync(\n      `git push origin ${tag} || (${getErrorCmd(\n        `Error pushing changes to tag ${tag}, ${checkLog}`\n      )})`,\n      { stdio: \"inherit\" }\n    );\n  }\n}\n\nfunction cloneRepo() {\n  execSync(\n    `\n    copado -p 'Cloning git repository'\n    copado-git-get ${branchName} || (${getErrorCmd(\n      `Error check out branch, ${checkLog}`\n    )})\n  `,\n    { stdio: \"inherit\" }\n  );\n}\n\nfunction setInstanceURL() {\n  execSync(\n    `\n    copado -p 'Setting instance url'\n    sfdx force:config:set instanceUrl=${baseUrl} --global || (${getErrorCmd(\n      `Error setting instance URL, ${checkLog}`\n    )})\n  `,\n    { stdio: \"inherit\" }\n  );\n}\n\nfunction setProgressStatus({ progressStatus, errorMessage, resultData}) {\n  execSync(`copado -p '${progressStatus}' ${errorMessage ? `-e '${errorMessage}'` : ''} ${resultData ? `-r '${resultData}'` : ''}`);\n  if(errorMessage)\n      throw errorMessage;\n}\n\nfunction getErrorCmd(error) {\n  return `copado -p 'Error' -e \"${error}\" && exit 1`;\n}\n\nfunction getUpdateParams(flags) {\n  return flags.reduce((parameters, obj)=>{\n      return parameters + (typeof obj.value == 'boolean' ? (obj.value ? ` --${obj.key}` : \"\" ) : (obj.value ? ( obj.isString ? ` --${obj.key} '${obj.value}'` : ` --${obj.key} ${obj.value}`) : ''));\n  },'')\n}",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1",
                    "copado__Worker_Size__c": "S",
                    "Id": "a0l7Q000000tRpUQAU",
                    "LastReferencedDate": "2022-07-05T09:36:03.000+0000",
                    "LastViewedDate": "2022-07-05T09:36:03.000+0000",
                    "Name": "SFDX Package Version Create"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0l7Q000000iAiRQAU"
                    },
                    "copado__API_Name__c": "SFDX_Get_Package_Version_Dependencies",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"endpoint\",\n  \"defaultValue\" : \"{$Destination.Credential.Endpoint}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"session\",\n  \"defaultValue\" : \"{$Destination.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"subscriberVersionId\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.subscriberId}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"installationKey\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.installationKey}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\nconst { execSync } = require('child_process'),\n\t{ endpoint, session, subscriberVersionId, installationKey } = process.env,\n\tbaseUrl = endpoint.substring(0, endpoint.indexOf('/', endpoint.indexOf('/') + 2)),\n\tcheckLog = \"Please check the logs for details\";\n\n// EXECUTON\n\nexecute();\n\nfunction execute() {\n\ttry {\n\t\tsetInstanceURL();\n\t\tconst dependencies = getVersionDependencies();\n\t\thandleDependenciesResponse(dependencies);\n\t} catch (err) {\n\t\texecSync(`${getErrorCmd(err.toString())}`);\n\t}\n}\n\n// SCRIPT FUNCTIONS\n\nfunction setInstanceURL() {\n\texecSync(\n\t\t`\n\t  copado -p 'Setting instance url'\n\t  sfdx force:config:set instanceUrl=${baseUrl} --global || (${getErrorCmd(\n\t\t\t`Error setting instance URL, ${checkLog}`\n\t\t)})\n\t`,\n\t\t{ stdio: \"inherit\" }\n\t);\n}\n\nfunction getVersionDependencies() {\n\tconst query = `SELECT Dependencies FROM SubscriberPackageVersion WHERE Id='${subscriberVersionId}' ${addFilter()}`;\n\tconst result = execSync(`\n\t\t\tcopado -p 'Getting dependencies'\n\t\t\tsfdx force:data:soql:query -t -q \"${query}\" -u ${session} --json || true\n\t\t`).toString();\n\treturn JSON.parse(result);\n}\n\nfunction getErrorCmd(error) {\n\treturn `copado -p 'Error' -e \"${error}\" && exit 1`;\n}\n\nfunction addFilter() {\n\treturn installationKey ? `AND InstallationKey='${installationKey}'` : '';\n}\n\nfunction handleDependenciesResponse(response) {\n\tlet updateResult;\n\tif (!response.status) {\n\t\tconst dependencies = response.result.records[0]?.Dependencies?.ids?.map(\n\t\t\tdependency => dependency.subscriberPackageVersionId\n\t\t);\n\t\tupdateResult = dependencies?.length ? `copado -p 'Updating dependencies' -r '${JSON.stringify(dependencies)}'` : '';\n\t} else {\n\t\tupdateResult = getErrorCmd(`${response.name}: ${response.message}`);\n\t}\n\tif (updateResult) {\n\t\texecSync(updateResult, {\n\t\t\tstdio: 'inherit'\n\t\t});\n\t}\n}",
                    "copado__Type__c": "Standard",
                    "copado__Worker_Size__c": "S",
                    "Id": "a0l7Q000000iAiRQAU",
                    "LastReferencedDate": "2022-07-05T10:07:06.000+0000",
                    "LastViewedDate": "2022-07-05T10:07:06.000+0000",
                    "Name": "SFDX Get Package Version Dependencies"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0k09000000qQSoAAM"
                    },
                    "copado__API_Name__c": "sfdx_execute_apex",
                    "copado__Description__c": "Run sfdx force:apex:execute to execute Apex Job Steps.",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"destination_sessionid\",\n  \"defaultValue\" : \"{$Destination.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"destination_endpoint\",\n  \"defaultValue\" : \"{$Destination.Credential.Endpoint}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"script\"\n}, {\n  \"required\" : true,\n  \"name\" : \"destination_env_var\",\n  \"defaultValue\" : \"{$Destination.apex.EnvironmentVariables}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"isValidation\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.deploymentDryRun}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n/**\n * Performs execution of apex script using sfdx cli command.\n * @param destination_sessionid\n * @param destination_endpoint\n * @param script\n * @param destination_env_var\n * @param isValidation\n */\n\nconst fs = require(\"fs\"),\n  { execSync } = require(\"child_process\"),\n  { destination_sessionid, destination_endpoint, script, destination_env_var, isValidation } = process.env,\n  destination_base_url = getBaseUrl(destination_endpoint),\n  scriptName = \"script.apex\",\n  validationModeMessage = \"This step will not be executed for validate changes\";\n\n// EXECUTION\n\ntry {\n  validateExecutionMode();\n  setup();\n  executeApexScript();\n} catch (error) {\n  logError(error);\n}\n\n// FUNCTIONS\n\nfunction getBaseUrl(endpoint) {\n  return endpoint.substring(\n    0,\n    endpoint.indexOf(\"/\", endpoint.indexOf(\"/\") + 2)\n  );\n}\n\nfunction setup() {\n  const content = script.replace(\"\\\\n\", \" \");\n  fs.writeFileSync(scriptName, content);\n  const cmd = `\n      copado -p \"Replacing environment dependent value\"\n\t  varreplace '${destination_env_var}' '${scriptName}' --valuename=false\n      copado -p 'Connecting to destination environment'\n      sfdx force:config:set instanceUrl=${destination_base_url} --global\n  `;\n  execSync(cmd, { stdio: \"inherit\" });\n}\n\nfunction executeApexScript() {\n  const cmd = `\n        copado -p 'Executing apex script'\n        sfdx force:apex:execute -u ${destination_sessionid} -f '${scriptName}' --json\n  `;\n  const response = execSync(cmd, { maxBuffer: 50 * 1024 * 1024 });\n  validateResponse(response);\n}\n\nfunction validateResponse(response) {\n  if (!response) {\n    throw \"Apex script execution failed\";\n  }\n  const formattedResponse = JSON.parse(response.toString());\n  if (formattedResponse.status != 0) {\n    throw formattedResponse?.message;\n  } else if (!formattedResponse?.result?.compiled || !formattedResponse?.result?.success) {\n    throw `Error occured: \"${JSON.stringify(formattedResponse?.result)}\". Please check logs for more details.`;\n  } else {\n    execSync(`copado -p 'Apex script execution completed successfully'`);\n  }\n}\n\nfunction logError(error) {\n  execSync(`copado -p 'Error' -e \"${error.toString()}\" && exit 1`);\n}\n\nfunction validateExecutionMode() {\n  if(isValidation === 'true') {\n    execSync(`copado -p '${validationModeMessage}' --result-data '${validationModeMessage}'`);\n\tprocess.exit(0);\n  }\n}",
                    "copado__Timeout__c": 60,
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1",
                    "copado__Worker_Size__c": "M",
                    "Id": "a0k09000000qQSoAAM",
                    "LastReferencedDate": "2022-06-27T07:54:21.000+0000",
                    "LastViewedDate": "2022-06-27T07:54:21.000+0000",
                    "Name": "SFDX Execute Apex"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v54.0/sobjects/copado__Function__c/a0l7Q000000MDZ4QAO"
                    },
                    "copado__API_Name__c": "SFDXPackageCreate",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"ENDPOINT\",\n  \"defaultValue\" : \"{$Destination.Credential.Endpoint}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"SESSION\",\n  \"defaultValue\" : \"{$Destination.Credential.SessionId}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"packageName\",\n  \"defaultValue\" : \"\"\n}, {\n  \"name\" : \"path\",\n  \"defaultValue\" : \"\"\n}, {\n  \"name\" : \"jsonInformation\",\n  \"defaultValue\" : \"\"\n}, {\n  \"name\" : \"description\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : true,\n  \"name\" : \"git_json\",\n  \"defaultValue\" : \"{$Context.Repository.Credential}\"\n}, {\n  \"name\" : \"branch\",\n  \"defaultValue\" : \"\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\nconst fs = require('fs');\n\nconst { execSync } = require('child_process'),\n    endPoint = process.env['ENDPOINT'],\n    branchName = process.env['branch'],\n    params = JSON.parse(process.env['jsonInformation'].replace(/\\\\/g, '')),\n    baseUrl = endPoint.substring(0, endPoint.indexOf('/', endPoint.indexOf('/') + 2)),\n    dirName = 'sfProject',\n    checkLog = 'Please check the logs for details';\n\n// EXECUTION\ntry {\n    const flags = `\n${getFlag('loglevel', params?.loglevel)}\n${getFlag('targetdevhubusername', process.env['SESSION'])}\n${getFlag('apiversion', params?.apiversion)}\n${getStringFlag('name', process.env['packageName'])}\n${getStringFlag('path', process.env['path'])}\n${getStringFlag('packagetype', params?.packagetype)}\n${getBooleanFlag('nonamespace', params?.nonamespace)}\n${getBooleanFlag('orgdependent', params?.orgdependent)}\n${getStringFlag('description', process.env['description'])}\n${getStringFlag('errornotificationusername', params?.errornotificationusername)}\n`;\n    fs.mkdirSync(dirName);\n    process.chdir(dirName);\n\n    prepareAndCloneRepo();\n    createPackage(flags);\n\n} catch (err) {\n    execSync(`${getErrorCmd(err.toString())}`);\n}\n\n\n// SCRIPT FUNCTIONS\nfunction getFlag(flagname, value) {\n\treturn value ? ` --${flagname} ${value}` : '';\n}\n\nfunction getStringFlag(flagname, value) {\n\treturn value ? ` --${flagname} \"${value}\"` : '';\n}\n\nfunction getBooleanFlag(flagname, value) {\n\treturn value ? ` --${flagname}` : '';\n}\n\nfunction prepareAndCloneRepo(){\n    execSync(`\n        copado -p 'Cloning git repository'\n        copado-git-get \"${branchName}\" || (${getErrorCmd(`Could not checkout branch ${branchName}, ${checkLog}`)})\n    `);\n\n    execSync(`\n        copado -p 'Checked out branch \"${branchName}\"'\n        copado -p 'Setting instance url'\n        sfdx force:config:set instanceUrl=${baseUrl} --global || (${getErrorCmd(`Failed setting an instance URL, ${checkLog}`)})\n    `);\n}\n\nfunction createPackage(pkgCreateParam){\n    const createCmd = `\n        copado -p 'Creating package'\n        sfdx force:package:create ${pkgCreateParam.replace(/\\n|\\r/g, '')} --json || true\n        copado -p 'Finishing'\n    `;\n\n    const response = JSON.parse(execSync(createCmd).toString());\n\n    if (!response?.status && response?.result?.Id) {    \n        commit();\n        setProgressStatus({ progressStatus: 'Package created successfully', resultData: `${response.result.Id}` });\n    }\n    else if (response?.status && response?.message)\n        setProgressStatus({ progressStatus: 'Package creation failed', errorMessage: `Could not create package, ${response.message}` });\n    else \n        setProgressStatus({ progressStatus: 'Package creation did not finish', resultData: `${JSON.stringify(response)}`, errorMessage: `Could not create package, ${response.message}` });\n}\n\nfunction commit(){\n    execSync(`\n        git add \"sfdx-project.json\" || (${getErrorCmd(`Error adding git change, ${checkLog}`)})\n        git commit -m \"Committing sfdx-project.json\" || (${getErrorCmd(`Error commiting git change, ${checkLog}`)})\n        git push origin ${branchName} || (${getErrorCmd(`Error pushing changes in git, ${checkLog}`)})`\n    , { stdio: 'inherit' });\n}\n\nfunction setProgressStatus({ progressStatus, errorMessage, resultData}) {\n    execSync(`copado -p '${progressStatus}' ${errorMessage ? `-e '${errorMessage}'` : ''} ${resultData ? `-r '${resultData}'` : ''}`);\n    if(errorMessage)\n        throw errorMessage;\n}\n\nfunction getErrorCmd(error) {\n    return `copado -p 'Error' -e \"${error}\" && exit 1`;\n}",
                    "copado__Type__c": "Standard",
                    "copado__Worker_Size__c": "S",
                    "Id": "a0l7Q000000MDZ4QAO",
                    "LastReferencedDate": "2022-05-10T06:56:31.000+0000",
                    "LastViewedDate": "2022-05-10T06:56:31.000+0000",
                    "Name": "SFDX Package Create"
                },
                {
                    "attributes": {
                        "type": "copado__Function__c",
                        "url": "/services/data/v54.0/sobjects/copado__Function__c/a0l7Q000000M5T0QAK"
                    },
                    "copado__API_Name__c": "SFDXRunApexTests",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"name\" : \"source_session_id\",\n  \"defaultValue\" : \"{$Source.Credential.SessionId}\"\n}, {\n  \"name\" : \"source_endpoint\",\n  \"defaultValue\" : \"{$Source.Credential.Endpoint}\"\n}, {\n  \"name\" : \"namespace\",\n  \"defaultValue\" : \"\"\n}, {\n  \"name\" : \"source_path\",\n  \"defaultValue\" : \"force-app\"\n}, {\n  \"name\" : \"test_minutes_timeout\",\n  \"defaultValue\" : \"1440\"\n}, {\n  \"name\" : \"test_result_ids\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.resultIds}\"\n}, {\n  \"name\" : \"test_classes\",\n  \"defaultValue\" : \"{$Context.apex.cmcSf.GetTestClasses}\"\n}, {\n  \"name\" : \"consolidated_result_id\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.resultId}\"\n} ]",
                    "copado__Script__c": "prepareSfdxProject() {\n\tcopado -p \"Preparing sfdx project configuration\"\n\n\tsource_endpoint=($(node -p \"('$source_endpoint').substring(0, '$source_endpoint'.indexOf('.com') + 4)\"))\n    \n    mkdir -p .sfdx\n\tcat << __EOF__ > \".sfdx/sfdx-config.json\"\n\t{\n\t\t\"instanceUrl\": \"$source_endpoint\"\n\t}\n__EOF__\n\t\n    cat << __EOF__ > \"sfdx-project.json\"\n\t{\n\t\t\"namespace\": \"$namespace\",\n\t\t\"packageDirectories\": [\n\t\t\t{\n\t\t\t\t\"path\": \"$source_path\",\n                \"default\": true\n            }\n\t\t],\n        \"sfdcLoginUrl\": \"\"\n\t}\n__EOF__\n}\n\nrunApexTests() {\n\techo \"test classes: ${test_classes}\"\n    copado -p \"Running Apex Tests in the source environment\"\n    sfdx force:apex:test:run -u ${source_session_id} -t ${test_classes} -r json --detailedcoverage --codecoverage --verbose --wait ${test_minutes_timeout} > /tmp/apex-tests-output.json || true\n    jq 'del(.result.summary.username)' /tmp/apex-tests-output.json > tmp.$$.json && mv tmp.$$.json /tmp/apex-tests-output.json\n}\n\nattachResultFile() {\n    # To function result\n\tcopado -u /tmp/apex-tests-output.json --name \"apex-tests-output.json\"\n\n\t# To test independent results, if existing\n\tif [ -n \"$test_result_ids\" ];\n    then\n      test_result_ids=$(echo $test_result_ids | sed -e \"s/\\[//g\" | sed -e \"s/\\]//g\" | sed -e \"s/ //g\" | xargs)\n      IFS=','\n      for test_result_id in ${test_result_ids};\n      do\n          copado -u /tmp/apex-tests-output.json --name \"apex-tests-output.json\" --parentid $test_result_id\n      done\n    fi\n    \n    # To consolidated result, if existing\n\tif [ -n \"$consolidated_result_id\" ];\n    then\n\t    copado -u /tmp/apex-tests-output.json --name \"apex-tests-output.json\" --parentid $consolidated_result_id\n    fi\n}\n\nprepareSfdxProject\nrunApexTests\nattachResultFile",
                    "copado__Timeout__c": 1440,
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1",
                    "copado__Worker_Size__c": "S",
                    "Id": "a0l7Q000000M5T0QAK",
                    "LastReferencedDate": "2022-04-21T16:43:31.000+0000",
                    "LastViewedDate": "2022-04-21T16:43:31.000+0000",
                    "Name": "SFDX Run Apex Tests"
                },
                {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0l7Q000000iFzHQAU"
                    },
                    "copado__API_Name__c": "SFDX_Package_Version_Git_Config",
                    "copado__Description__c": "SFDX Package Version Git Config",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : false,\n  \"name\" : \"packageVersion\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.packageVersion}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"git_json\",\n  \"defaultValue\" : \"{$Context.Repository.Credential}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n\nconst { readFileSync, mkdirSync } = require(\"fs\"),\n\t{ execSync } = require(\"child_process\"),\n\t{ env, chdir } = require(\"process\"),\n\tcheckLog = \"Please check the logs for details\",\n\tprojectDirectory = \"sfProject\",\n\tpackageVersion = JSON.parse(env.packageVersion);\n\n// EXECUTION\n\ntry {\n\tmkdirSync(projectDirectory);\n\tchdir(projectDirectory);\n\tcloneRepo(packageVersion.copado__Branch__c);\n\texecSync(`copado -p 'Fetching details from repository'`);\n\tconst result = getVersionDetailsResult(readProjectJson());\n\n\texecSync(\n\t\t`copado -p \"Fetch Successful\" -r '${result}' && exit 0`,\n\t\t{ stdio: \"inherit\" }\n\t);\n} catch (err) {\n\texecSync(`${getErrorCmd(err.toString())}`);\n}\n\n// SCRIPT FUNCTIONS\n\nfunction cloneRepo(branchName) {\n\texecSync(\n\t\t`\n\t\tcopado -p 'Cloning git repository'\n\t\tcopado-git-get ${branchName} --depth 1 || (${getErrorCmd(\n\t\t\t`Error check out branch, ${checkLog}`\n\t\t)})`,\n\t\t{ stdio: \"inherit\" }\n\t);\n}\n\nfunction readProjectJson() {\n\treturn JSON.parse(readFileSync(\"sfdx-project.json\", \"utf-8\"));\n}\n\nfunction getErrorCmd(error) {\n\treturn `copado -p 'Error' -e \"${error}\" && exit 1`;\n}\n\nfunction getVersionDetailsResult(projectJson) {\n\tconst packageId = packageVersion?.copado__Artifact__r?.copado__Package_Id__c,\n\t\tpackageAliases = projectJson?.packageAliases,\n\t\trecordNamespace = packageVersion.copado__Artifact__r?.copado__Package_Namespace__c,\n\t\tprojectJsonNamespace = projectJson?.namespace,\n\t\tpkgName = Object.keys(packageAliases).find(pckgId => packageAliases[pckgId] === packageId);\n\n\tcheckAttributesAndHandleErrors({ packageAliases, packageId, pkgName, recordNamespace, projectJsonNamespace });\n\tconst pkgDirectory = projectJson?.packageDirectories.find(\n\t\tdir => dir.package === pkgName\n\t),\n\t\t{ versionNumber, versionDescription, versionName, postInstallUrl, ancestorVersion, ancestorId, postInstallScript, releaseNotesUrl, uninstallScript, definitionFile } = pkgDirectory;\n\n\n\tif (!pkgDirectory) {\n\t\texecSync(\n\t\t\t`${getErrorCmd(`Could not find pkgDirectory attribute in project JSON file.`)}`\n\t\t);\n\t}\n\treturn JSON.stringify({\n\t\tpackageVersion: {\n\t\t\tId: packageVersion.Id,\n\t\t\tcopado__Version_number__c: versionNumber,\n\t\t\tcopado__Version_Description__c: versionDescription,\n\t\t\tcopado__Version_Name__c: versionName,\n\t\t\tPost_Install_URL__c: postInstallUrl,\n\t\t\tPost_Install_Script__c: postInstallScript,\n\t\t\tRelease_Notes_URL__c: releaseNotesUrl,\n\t\t\tUninstall_Script__c: uninstallScript,\n\t\t\tcopado__DefinitionFile__c: definitionFile,\n\t\t\tAncestor_Version__c: ancestorVersion,\n\t\t\tAncestor_Id__c: packageAliases[ancestorId]\n\t\t}\n\t});\n}\n\nfunction checkAttributesAndHandleErrors({ packageAliases, packageId, pkgName, recordNamespace, projectJsonNamespace }) {\n\tif (!packageAliases) {\n\t\texecSync(\n\t\t\t`${getErrorCmd(`Missing attribute packageAliases from project JSON file`)}`\n\t\t);\n\t}\n\n\tif (!packageId || !pkgName) {\n\t\texecSync(\n\t\t\t`${getErrorCmd(`Missing associated package either in Salesforce record or project JSON file.`)}`\n\t\t);\n\t}\n\n\tif (recordNamespace && projectJsonNamespace && recordNamespace != projectJsonNamespace) {\n\t\texecSync(\n\t\t\t`${getErrorCmd(`Mismatch of namespace in Package record and repository.`)}`\n\t\t);\n\t}\n}",
                    "copado__Timeout__c": 120,
                    "copado__Type__c": "Standard",
                    "copado__Worker_Size__c": "S",
                    "Id": "a0l7Q000000iFzHQAU",
                    "LastReferencedDate": "2022-06-27T09:30:36.000+0000",
                    "LastViewedDate": "2022-06-27T09:30:36.000+0000",
                    "Name": "SFDX Package Version Git Config"
                  },
                  {
                    "attributes": {
                      "type": "copado__Function__c",
                      "url": "/services/data/v55.0/sobjects/copado__Function__c/a0k09000000rbjMAAQ"
                    },
                    "copado__API_Name__c": "SFDX_Deploy_Custom_Setting",
                    "copado__Description__c": "This function is used to create custom setting records",
                    "copado__Image_Name__c": "copado-multicloud-dx:v3",
                    "copado__Options__c": "[ ]",
                    "copado__Parameters__c": "[ {\n  \"required\" : true,\n  \"name\" : \"source_sessionid\",\n  \"defaultValue\" : \"{$Source.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"source_endpoint\",\n  \"defaultValue\" : \"{$Source.Credential.Endpoint}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"source_env_var\",\n  \"defaultValue\" : \"{$Source.apex.EnvironmentVariables}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"destination_env_var\",\n  \"defaultValue\" : \"{$Destination.apex.EnvironmentVariables}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"destination_sessionid\",\n  \"defaultValue\" : \"{$Destination.Credential.SessionId}\"\n}, {\n  \"required\" : true,\n  \"name\" : \"destination_endpoint\",\n  \"defaultValue\" : \"{$Destination.Credential.Endpoint}\"\n}, {\n  \"required\" : false,\n  \"name\" : \"records\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : false,\n  \"name\" : \"custom_setting_name\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : false,\n  \"name\" : \"profiles\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : false,\n  \"name\" : \"users\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : false,\n  \"name\" : \"organizations\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : false,\n  \"name\" : \"key_prefix\",\n  \"defaultValue\" : \"\"\n}, {\n  \"required\" : true,\n  \"name\" : \"isValidation\",\n  \"defaultValue\" : \"{$Context.JobExecution__r.DataJson.deploymentDryRun}\"\n} ]",
                    "copado__Script__c": "#!/usr/bin/env node\n/**\n * Performs deployment of custom setting records.\n * @param source_sessionid\n * @param source_endpoint\n * @param source_env_var\n * @param destination_env_var\n * @param destination_sessionid\n * @param destination_endpoint\n * @param records\n * @param custom_setting_name\n * @param profiles\n * @param users\n * @param organizations\n * @param key_prefix\n * @param isValidation\n */\n\nconst fs = require('fs'),\n    { execSync, exec } = require('child_process'),\n    {\n        source_sessionid,\n        source_endpoint,\n        source_env_var,\n        destination_env_var,\n        destination_sessionid,\n        destination_endpoint,\n        custom_setting_name,\n        isValidation\n    } = process.env,\n    sourceApiVersion = '55.0',\n    source_base_url = getBaseUrl(source_endpoint),\n    destination_base_url = getBaseUrl(destination_endpoint),\n    source_environment = 'source',\n    destination_environment = 'destination';\n\nlet { records, profiles, users, organizations, key_prefix } = process.env,\n    profileQuery,\n    userQuery,\n    organizationQuery,\n    recordNames,\n    sourceCustomSetting = {},\n    sourceRecords = {\n        profileMapping: {},\n        userMapping: {},\n        organizationMapping: {},\n        records: []\n    },\n    destinationRecords = {\n        profileMapping: {},\n        userMapping: {},\n        organizationMapping: {},\n        records: {}\n    };\n\n// EXECUTION\n\nexecute();\n\n// FUNCTIONS\n\nfunction execute() {\n    try {\n        validateExecutionMode();\n        setup();\n        validateSource();\n        prepareQueryForDependentRecords();\n        Promise.all(getCustomSettingDependentData(source_sessionid, `Retrieve records from ${source_environment}`))\n            .then((data) => {\n                queryCustomSettingInSource(data);\n                validateDestination();\n                Promise.all(getCustomSettingDependentData(destination_sessionid, `Retrieve records from ${destination_environment}`))\n                    .then((data) => {\n                        queryCustomSettingInDestination(data);\n                        mergeAndDeployRecords();\n                    })\n                    .catch((error) => {\n                        logError(error);\n                    });\n            })\n            .catch((error) => {\n                logError(error);\n            });\n    } catch (error) {\n        logError(error);\n    }\n}\n\nfunction getBaseUrl(endpoint) {\n    return endpoint.substring(0, endpoint.indexOf('/', endpoint.indexOf('/') + 2));\n}\n\nfunction validateExecutionMode() {\n    const validationModeMessage = 'This step will not be executed for validate changes';\n    if (isValidation === 'true') {\n        execSync(`copado -p '${validationModeMessage}' --result-data '${validationModeMessage}'`);\n        process.exit(0);\n    }\n}\n\nfunction logError(error) {\n    execSync(`copado -p 'Error' -e '${error.toString()}' && exit 1`);\n}\n\nfunction setup() {\n    const cmd = `\n copado -p 'Preparing sfdx project configuration'\n cd /app\n sfdx force:project:create --projectname 'sfdx_project' --json\n `;\n    execSync(cmd, { stdio: 'inherit' });\n    let projectJson = fs.readFileSync('/app/sfdx_project/sfdx-project.json')?.toString();\n    if (projectJson) {\n        projectJson = JSON.parse(projectJson);\n        if(projectJson.sourceApiVersion !== sourceApiVersion) {\n        \tprojectJson = { ...projectJson, sourceApiVersion };\n        \tfs.writeFileSync('/app/sfdx_project/sfdx-project.json', JSON.stringify(projectJson));\n        }\n    }\n}\n\nfunction setInstanceUrl(url, message) {\n    const cmd = `\n copado -p '${message}'\n cd /app/sfdx_project\n sfdx config:set instanceUrl=${url}\n `;\n    execSync(cmd, { stdio: 'inherit' });\n}\n\nfunction retrieveCustomSettingMetadata(sessionid, message, environment) {\n    let result;\n    if (!custom_setting_name) {\n        throw `Error occured (${environment}), invalid custom setting name. Please check logs for more details.`;\n    }\n    const cmd = `\n copado -p '${message}'\n cd /app/sfdx_project\n sfdx force:source:retrieve --metadata CustomObject:${custom_setting_name} -u ${sessionid} --json`;\n    const response = execSync(cmd);\n    if (!response) {\n        throw `Error occured (${environment}), custom setting metadata retrieval failed. Please check logs for more details.`;\n    } else {\n        result = extractCustomSettingMetadata(JSON.parse(response.toString()), environment);\n    }\n    return result;\n}\n\nfunction extractCustomSettingMetadata(metadataResponse, environment) {\n    const result = { fields: [], filePath: '' };\n    const metadata = metadataResponse?.result?.inboundFiles;\n    if (metadataResponse.status != 0 || !metadata) {\n        throw `Error occured (${environment}), '${JSON.stringify(metadataResponse)}'. Please check logs for more details.`;\n    } else {\n        metadata.forEach((element) => {\n            if (element.error) {\n                throw `Error occured (${environment}), '${JSON.stringify(element.error)}'. Please check logs for more details.`;\n            } else if (element.type === 'CustomField') {\n                result.fields.push(element.fullName.replace(`${custom_setting_name}.`, ''));\n            } else if (element.type === 'CustomObject') {\n                result.filePath = element.filePath;\n            }\n        });\n    }\n    return result;\n}\n\nfunction checkCustomSettingTypeAndVisibility(customSettingPath, message, environment) {\n    const result = { isHierarchy: false };\n    if (!customSettingPath) {\n        throw `Error occured (${environment}), invalid custom setting filePath. Please check logs for more details.`;\n    }\n    const cmd = `\n copado -p '${message}'\n cd /app/sfdx_project\n cat ${customSettingPath}\n `;\n    let response = execSync(cmd);\n    if (!response) {\n        throw `Error occured (${environment}), custom setting details not found. Please check logs for more details.`;\n    } else {\n        response = response.toString();\n        if (!response.includes('<visibility>Public</visibility>')) {\n            throw `Error occured (${environment}), '${JSON.stringify(\n                response\n            )}'. Only public custom setting records can be moved from source org to destination org. Please check logs for more details.`;\n        }\n        result.isHierarchy = response.includes('<customSettingsType>Hierarchy</customSettingsType>');\n    }\n    return result;\n}\n\nfunction validateSource() {\n    setInstanceUrl(source_base_url, `Setting instance url to ${source_environment}`);\n    const metadata = retrieveCustomSettingMetadata(\n        source_sessionid,\n        `Retrieve and validate custom setting in ${source_environment}`,\n        source_environment\n    );\n    Object.assign(sourceCustomSetting, metadata);\n    const type = checkCustomSettingTypeAndVisibility(\n        sourceCustomSetting.filePath,\n        `Checking custom setting type and visibility in ${source_environment}`,\n        source_environment\n    );\n    Object.assign(sourceCustomSetting, type);\n}\n\nfunction prepareQueryForDependentRecords() {\n    const hierarchyCustomSettingError = `Error occured, for hierarchy custom setting we need profile, user or organization record(s) to deploy the custom setting data. Please check logs for more details.`;\n    const listCustomSettingError = `Error occured, no custom setting record(s) found for deployment. Please check logs for more details.`;\n    if (sourceCustomSetting.isHierarchy) {\n        if (!profiles && !users && !organizations) {\n            throw hierarchyCustomSettingError;\n        }\n        profiles = JSON.parse(profiles);\n        users = JSON.parse(users);\n        organizations = JSON.parse(organizations);\n        if (profiles?.length) {\n            profileQuery = `SELECT Id, Name FROM Profile WHERE Name IN ('${profiles.map((profile) => profile.Name).join(\"','\")}')`;\n        }\n        if (users?.length) {\n            const userNames = [];\n            const userProfileNames = [];\n            users.forEach((user) => {\n                userNames.push(user.Name);\n                userProfileNames.push(user.Profile.Name);\n            });\n            userQuery = `SELECT Id, Name FROM User WHERE Name IN ('${userNames.join(\"','\")}') AND Profile.Name IN ('${userProfileNames.join(\n                \"','\"\n            )}')`;\n        }\n        if (organizations?.length) {\n            organizationQuery = `SELECT Id, Name FROM Organization WHERE Name IN ('${organizations\n                .map((organization) => organization.Name)\n                .join(\"','\")}')`;\n        }\n        if (!profileQuery && !userQuery && !organizationQuery) {\n            throw hierarchyCustomSettingError;\n        }\n    } else if (!records) {\n        throw listCustomSettingError;\n    } else {\n        records = JSON.parse(records);\n        if (!records?.length) {\n            throw listCustomSettingError;\n        } else {\n            recordNames = records.map((record) => record.Name);\n        }\n    }\n}\n\nfunction queryCustomSettingInSource(data) {\n    const filterResponse = prepareCustomSettingQueryFilter(data, sourceCustomSetting.isHierarchy, 'Id', 'Name', source_environment);\n    Object.assign(sourceRecords, filterResponse);\n    delete sourceRecords.customSettingNames;\n    const queryResponse = queryCustomSettingRecords(\n        sourceCustomSetting.isHierarchy,\n        Object.keys(sourceRecords.profileMapping),\n        Object.keys(sourceRecords.userMapping),\n        Object.keys(sourceRecords.organizationMapping),\n        filterResponse.customSettingNames,\n        `Error occured, no filters avaliable to query custom seting records in ${source_environment}. Please check logs for more details.`,\n        sourceCustomSetting.fields,\n        source_sessionid,\n        source_environment\n    );\n    sourceRecords.records = getCustomSettingRecords(\n        JSON.parse(queryResponse),\n        source_environment,\n        `Error occured, there are no custom setting records available in ${source_environment} as per the mentioned filters. Please check logs for more details.`\n    );\n}\n\nfunction getCustomSettingRecords(data, environment, noRecordsError) {\n    let result;\n    if (data.status != 0) {\n        throw `Error occured (${environment}), '${JSON.stringify(data)}' Please check logs for more details.`;\n    } else {\n        result = data?.result?.records;\n        if (!records?.length && noRecordsError) {\n            throw noRecordsError;\n        }\n    }\n    return result;\n}\n\nfunction prepareCustomSettingQueryFilter(data, isHierarchy, key, value, environment) {\n    const result = { profileMapping: {}, userMapping: {}, organizationMapping: {}, customSettingNames: null };\n    data.forEach((response) => {\n        if (isHierarchy) {\n            response = JSON.parse(response);\n            if (response.status != 0) {\n                throw `Error occured (${environment}), '${JSON.stringify(response)}'. Please check logs for more details.`;\n            } else {\n                const records = response?.result?.records;\n                if (records?.length) {\n                    mapRecords(records, result, key, value);\n                }\n            }\n        } else {\n            result.customSettingNames = response;\n        }\n    });\n    return result;\n}\n\nfunction queryCustomSettingRecords(\n    isHierarchy,\n    profileIds,\n    userIds,\n    orgIds,\n    customSettingNames,\n    noFilterError,\n    fieldsToQuery,\n    sessionid,\n    environment\n) {\n    let response;\n    let filters = [];\n    if (isHierarchy) {\n        filters = filters.concat(profileIds, userIds, orgIds);\n    } else {\n        filters = filters.concat(customSettingNames);\n    }\n\n    if (!filters?.length) {\n        if (noFilterError) {\n            throw noFilterError;\n        }\n    } else {\n        const filterIds = `('${filters.join(\"','\")}')`;\n        const query = `SELECT Id, Name, SetupOwnerId ${\n            fieldsToQuery?.length ? ',' + fieldsToQuery.join(',') : ''\n        } FROM ${custom_setting_name} WHERE ${isHierarchy ? 'SetupOwnerId IN ' : 'Name IN '} ${filterIds}`;\n        response = doSoqlSyncQuery(query, sessionid);\n    }\n\n    if (!response) {\n        throw `Error occured (${environment}), force:data:soql:query command failed. Please check logs for more details.`;\n    }\n    return response;\n}\n\nfunction mapRecords(records, dataSet, key, value) {\n    records.forEach((record) => {\n        switch (record?.attributes?.type) {\n            case 'Profile':\n                dataSet.profileMapping[record[key]] = record[value];\n                break;\n            case 'User':\n                dataSet.userMapping[record[key]] = record[value];\n                break;\n            case 'Organization':\n                dataSet.organizationMapping[record[key]] = record[value];\n                break;\n        }\n    });\n}\n\nfunction getCustomSettingDependentData(sessionId, message) {\n    exec(`copado -p '${message}'`, { stdio: 'inherit' });\n    const promises = [];\n    if (sourceCustomSetting.isHierarchy) {\n        [profileQuery, userQuery, organizationQuery].forEach((query) => {\n            if (query) {\n                promises.push(\n                    new Promise((resolve, reject) => {\n                        doSoqlAsyncQuery(resolve, reject, query, sessionId);\n                    })\n                );\n            }\n        });\n    } else {\n        promises.push(\n            new Promise((resolve, reject) => {\n                resolve(recordNames);\n            })\n        );\n    }\n    return promises;\n}\n\nfunction doSoqlAsyncQuery(resolve, reject, query, sessionId) {\n    exec(getQueryCmd(query, sessionId), (error, data, stderr) => {\n        if (error) {\n            reject(error);\n        } else {\n            resolve(data);\n        }\n    });\n}\n\nfunction doSoqlSyncQuery(query, sessionId) {\n    const response = execSync(getQueryCmd(query, sessionId));\n    if (!response) {\n        throw `Error occured, force:data:soql:query command failed. Please check logs for more details.`;\n    } else {\n        return response.toString();\n    }\n}\n\nfunction getQueryCmd(query, sessionId) {\n    return `\n cd /app/sfdx_project\n sfdx force:data:soql:query --query \"${query}\" --json -u ${sessionId}\n `;\n}\n\nfunction validateDestination() {\n    setInstanceUrl(destination_base_url, `Setting instance url to ${destination_environment}`);\n    const metadata = retrieveCustomSettingMetadata(\n        destination_sessionid,\n        `Retrieve and validate custom setting in ${destination_environment}`,\n        destination_environment\n    );\n    const missingFields = sourceCustomSetting.fields.filter((field) => metadata.fields.indexOf(field) === -1);\n    if (missingFields?.length) {\n        throw `Error occured, fields missing in ${destination_environment} ${missingFields.join(',')}. Please check logs for more details.`;\n    }\n    const type = checkCustomSettingTypeAndVisibility(\n        metadata.filePath,\n        `Checking custom setting type and visibility in ${destination_environment}`,\n        destination_environment\n    );\n    if (sourceCustomSetting.isHierarchy != type.isHierarchy) {\n        throw `Error occured, there is a mismatch in the custom setting type for source and destination. Please check logs for more details.`;\n    }\n}\n\nfunction queryCustomSettingInDestination(data) {\n    const filterResponse = prepareCustomSettingQueryFilter(data, sourceCustomSetting.isHierarchy, 'Name', 'Id', destination_environment);\n    Object.assign(destinationRecords, filterResponse);\n    delete destinationRecords.customSettingNames;\n    const queryResponse = queryCustomSettingRecords(\n        sourceCustomSetting.isHierarchy,\n        Object.values(destinationRecords.profileMapping),\n        Object.values(destinationRecords.userMapping),\n        Object.values(destinationRecords.organizationMapping),\n        filterResponse.customSettingNames,\n        null,\n        null,\n        destination_sessionid,\n        destination_environment\n    );\n    const records = getCustomSettingRecords(JSON.parse(queryResponse), destination_environment, null);\n    records.forEach((record) => {\n        destinationRecords.records[sourceCustomSetting.isHierarchy ? record.SetupOwnerId : record.Name] = record.Id;\n    });\n}\n\nfunction mergeAndDeployRecords() {\n    const finalRecords = mergeRecords();\n    createCSV(finalRecords);\n    varreplace();\n    deployRecords();\n}\n\nfunction mergeRecords() {\n    const finalRecords = [];\n    key_prefix = JSON.parse(key_prefix);\n    let destinationSetupOwnerId;\n    sourceRecords.records.forEach((record) => {\n        if (sourceCustomSetting.isHierarchy) {\n            destinationSetupOwnerId = null;\n            switch (key_prefix[record.SetupOwnerId.substring(0, 3)]) {\n                case 'Profile':\n                    destinationSetupOwnerId = destinationRecords.profileMapping[sourceRecords.profileMapping[record.SetupOwnerId]];\n                    break;\n                case 'User':\n                    destinationSetupOwnerId = destinationRecords.userMapping[sourceRecords.userMapping[record.SetupOwnerId]];\n                    break;\n                case 'Organization':\n                    destinationSetupOwnerId =\n                        destinationRecords.organizationMapping[sourceRecords.organizationMapping[record.SetupOwnerId]];\n                    break;\n            }\n            if (!destinationSetupOwnerId) {\n                return;\n            }\n        } else {\n            destinationSetupOwnerId = null;\n        }\n        delete record.attributes;\n        record.SetupOwnerId = destinationSetupOwnerId;\n        record.Id = sourceCustomSetting.isHierarchy\n            ? destinationRecords.records[destinationSetupOwnerId]\n            : destinationRecords.records[record.Name];\n        finalRecords.push(record);\n    });\n    return finalRecords;\n}\n\nfunction createCSV(finalRecords) {\n    if (!finalRecords?.length) {\n        throw `No custom setting records avaliable for deployment in ${destination_environment}`;\n    } else {\n        const csvData = arrayToCSV(finalRecords);\n        fs.writeFileSync('/tmp/records.csv', csvData);\n    }\n}\n\nfunction arrayToCSV(data) {\n    let csv = data.map((row) => Object.values(row));\n    csv.unshift(Object.keys(data[0]));\n    return `\"${csv.join('\"\\n\"').replace(/,/g, '\",\"')}\"`;\n}\n\nfunction varreplace() {\n    const cmd = `\n copado -p \"Replacing environment variables, if any\"\n varreplace '${source_env_var}' '/tmp/records.csv' --valuename=true\n varreplace '${destination_env_var}' '/tmp/records.csv' --valuename=false\n `;\n    execSync(cmd);\n}\n\nfunction deployRecords() {\n    sourceCustomSetting = sourceRecords = destinationRecords = profileQuery = userQuery = organizationQuery = recordNames = null;\n    const cmd = `\n cd /app/sfdx_project\n copado -p 'Deploying custom setting records in ${destination_environment}'\n sfdx force:data:bulk:upsert --sobjecttype ${custom_setting_name} --csvfile /tmp/records.csv --externalid Id -u ${destination_sessionid} --wait 60 > /tmp/result.txt\n copado -u /tmp/records.csv --name \"records.csv\"\n copado -u /tmp/result.txt --name \"result.txt\"\n `;\n    execSync(cmd, { maxBuffer: 50 * 1024 * 1024 });\n    const result = fs.readFileSync('/tmp/result.txt');\n    if (!result) {\n        throw 'Error occured while deploying records in destination, kindly check logs for more details';\n    } else if (result.includes('Upsert errors')) {\n        throw 'Error occured, kindly check the result.txt file for more details';\n    }\n}",
                    "copado__Timeout__c": 60,
                    "copado__Type__c": "Standard",
                    "copado__Version__c": "1",
                    "copado__Worker_Size__c": "M",
                    "Id": "a0k09000000rbjMAAQ",
                    "LastReferencedDate": "2022-06-27T07:34:47.000+0000",
                    "LastViewedDate": "2022-06-27T07:34:47.000+0000",
                    "Name": "SFDX Deploy Custom Setting"
                  }
            ],
            "ObjectType": "copado__Function__c"
        },
        {
            "Records": [
                {
                    "attributes": {
                      "type": "copado__JobTemplate__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobTemplate__c/a0u7Q000000Xw28QAC"
                    },
                    "copado__ApiName__c": "sfdx_commit_1",
                    "copado__Description__c": "<p>Standard job template to perform git commit in User Stories</p>",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 2,
                    "Id": "a0u7Q000000Xw28QAC",
                    "LastReferencedDate": "2022-07-27T16:46:35.000+0000",
                    "LastViewedDate": "2022-07-27T16:46:35.000+0000",
                    "Name": "SFDX Commit"
                },
                {
                    "attributes": {
                      "type": "copado__JobTemplate__c",
                      "url": "/services/data/v54.0/sobjects/copado__JobTemplate__c/a0u7Q000000Xw29QAC"
                    },
                    "copado__ApiName__c": "sfdx_deploy_1",
                    "copado__Description__c": "<p>DX Source Deployment</p>",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "copado__VolumeOptions__c": "[\n    {\n        \"name\": \"volumeSize\",\n        \"value\": \"10\"\n    },\n    {\n        \"name\": \"volumeTTL\",\n        \"value\": \"100\"\n    },\n    {\n        \"name\": \"preserveVolumeAfterExecution\",\n        \"value\": \"true\"\n    },\n    {\n        \"name\": \"volumeEnabled\",\n        \"value\": \"true\"\n    }\n]",
                    "Id": "a0u7Q000000Xw29QAC",
                    "LastReferencedDate": "2022-05-10T06:56:31.000+0000",
                    "LastViewedDate": "2022-05-10T06:56:31.000+0000",
                    "Name": "SFDX Deploy"
                },
                {
                    "attributes": {
                      "type": "copado__JobTemplate__c",
                      "url": "/services/data/v54.0/sobjects/copado__JobTemplate__c/a0u7Q000000Xw2AQAS"
                    },
                    "copado__ApiName__c": "sfdx_promote_1",
                    "copado__Description__c": "<p>DX Source Promote</p>",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "Id": "a0u7Q000000Xw2AQAS",
                    "LastReferencedDate": "2022-05-10T06:56:31.000+0000",
                    "LastViewedDate": "2022-05-10T06:56:31.000+0000",
                    "Name": "SFDX Promote"
                },
                {
                    "attributes": {
                      "type": "copado__JobTemplate__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobTemplate__c/a0u7Q0000004wGoQAI"
                    },
                    "copado__ApiName__c": "SFDX Package Distribution_1",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "Id": "a0u7Q0000004wGoQAI",
                    "LastReferencedDate": "2022-07-12T12:49:35.000+0000",
                    "LastViewedDate": "2022-07-12T12:49:35.000+0000",
                    "Name": "SFDX Package Distribution"
                },
                {
                    "attributes": {
                        "type": "copado__JobTemplate__c",
                        "url": "/services/data/v52.0/sobjects/copado__JobTemplate__c/a0t09000002vfccAAA"
                    },
                    "copado__ApiName__c": "SFDX Package Import_1",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "CreatedDate": "2021-09-17T14:07:46.000+0000",
                    "Id": "a0t09000002vfccAAA",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-04T09:11:43.000+0000",
                    "LastReferencedDate": "2021-10-04T09:11:43.000+0000",
                    "LastViewedDate": "2021-10-04T09:11:43.000+0000",
                    "Name": "SFDX Package Import",
                    "SystemModstamp": "2021-10-04T09:11:43.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobTemplate__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobTemplate__c/a0t09000002vumlAAA"
                    },
                    "copado__ApiName__c": "SFDX Package Version Publish_1",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "CreatedDate": "2021-09-30T20:25:20.000+0000",
                    "Id": "a0t09000002vumlAAA",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-04T09:11:43.000+0000",
                    "LastReferencedDate": "2021-10-28T14:46:22.000+0000",
                    "LastViewedDate": "2021-10-28T14:46:22.000+0000",
                    "Name": "SFDX Package Version Publish",
                    "SystemModstamp": "2021-10-04T09:11:43.000+0000"
                },
                {
                    "attributes": {
                      "type": "copado__JobTemplate__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobTemplate__c/a0u7Q0000004wGrQAI"
                    },
                    "copado__ApiName__c": "SFDX Package Version Update_1",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "Id": "a0u7Q0000004wGrQAI",
                    "LastReferencedDate": "2022-07-12T09:02:37.000+0000",
                    "LastViewedDate": "2022-07-12T09:02:37.000+0000",
                    "Name": "SFDX Package Version Update"
                },
                {
                    "attributes": {
                        "type": "copado__JobTemplate__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobTemplate__c/a0t09000002vuuvAAA"
                    },
                    "copado__ApiName__c": "SFDX Package Version Create_1",
                    "copado__Description__c": "<p>Creates Package Version</p>",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "CreatedDate": "2021-10-01T12:18:40.000+0000",
                    "Id": "a0t09000002vuuvAAA",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-27T11:24:47.000+0000",
                    "LastReferencedDate": "2021-11-08T09:25:55.000+0000",
                    "LastViewedDate": "2021-11-08T09:25:55.000+0000",
                    "Name": "SFDX Package Version Create",
                    "SystemModstamp": "2021-10-27T11:24:47.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobTemplate__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobTemplate__c/a0t09000002wVAsAAM"
                    },
                    "copado__ApiName__c": "SFDX Package Create_1",
                    "copado__Description__c": "<p>Creates Package</p>",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "CreatedDate": "2021-11-23T08:05:48.000+0000",
                    "Id": "a0t09000002wVAsAAM",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-11-23T08:05:48.000+0000",
                    "LastReferencedDate": "2022-01-08T18:48:46.000+0000",
                    "LastViewedDate": "2022-01-08T18:48:46.000+0000",
                    "Name": "SFDX Package Create",
                    "SystemModstamp": "2021-11-23T08:05:48.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobTemplate__c",
                        "url": "/services/data/v54.0/sobjects/copado__JobTemplate__c/a0u7Q000000XrrkQAC"
                    },
                    "copado__ApiName__c": "SFDX Run Apex Tests_1",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "Id": "a0u7Q000000XrrkQAC",
                    "LastReferencedDate": "2022-04-21T10:30:02.000+0000",
                    "LastViewedDate": "2022-04-21T10:30:02.000+0000",
                    "Name": "SFDX Run Apex Tests"
                },
                {
                    "attributes": {
                      "type": "copado__JobTemplate__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobTemplate__c/a0u7Q0000004yiqQAA"
                    },
                    "copado__ApiName__c": "SFDX Package Version Git Configure_1",
                    "copado__Description__c": "<p><span style=\"font-size: 14px;\">SFDX Package Version Git Configure</span></p>",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "Id": "a0u7Q0000004yiqQAA",
                    "LastReferencedDate": "2022-06-16T18:10:58.000+0000",
                    "LastViewedDate": "2022-06-16T18:10:58.000+0000",
                    "Name": "SFDX Package Version Git Configure"
                },
                {
                    "attributes": {
                      "type": "copado__JobTemplate__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobTemplate__c/a0t09000000lkYtAAI"
                    },
                    "copado__ApiName__c": "SFDX Test Records Creation In User Story_1",
                    "copado__Type__c": "Standard",
                    "copado__Version__c": 1,
                    "Id": "a0t09000000lkYtAAI",
                    "LastReferencedDate": "2022-07-11T06:25:51.000+0000",
                    "LastViewedDate": "2022-07-11T06:25:51.000+0000",
                    "Name": "SFDX Test Records Creation In User Story"
                }
            ],
            "ObjectType": "copado__JobTemplate__c"
        },
        {
            "Records": [
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000001EqBIQA0"
                    },
                    "copado__ApiName__c": "SFDX Package Distribution_Install package",
                    "copado__ConfigJson__c": "{\"functionName\":\"sfdx_install_package\",\"parameters\":[{\"name\":\"packages\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\",\"required\":true},{\"name\":\"baseUrl\",\"value\":\"{$Destination.Credential.Endpoint}\",\"required\":true},{\"name\":\"session\",\"value\":\"{$Destination.Credential.SessionId}\",\"required\":true},{\"name\":\"wait\",\"value\":\"200\",\"required\":false}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q0000004wGoQAI",
                    "copado__Order__c": 3,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000001EqBIQA0",
                    "Name": "Install package"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000001EqBJQA0"
                    },
                    "copado__ApiName__c": "SFDX Package Distribution_Get Dependencies",
                    "copado__ConfigJson__c": "{\"functionName\":\"SFDX_Get_Package_Version_Dependencies\",\"parameters\":[{\"name\":\"endpoint\",\"value\":\"{$Destination.Credential.Endpoint}\",\"required\":true},{\"name\":\"session\",\"value\":\"{$Destination.Credential.SessionId}\",\"required\":true},{\"name\":\"subscriberVersionId\",\"value\":\"{$Context.JobExecution__r.DataJson.subscriberId}\",\"required\":true},{\"name\":\"installationKey\",\"value\":\"{$Context.JobExecution__r.DataJson.installationKey}\",\"required\":false}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q0000004wGoQAI",
                    "copado__Order__c": 1,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000001EqBJQA0",
                    "Name": "Get Dependencies"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000001EqBKQA0"
                    },
                    "copado__ApiName__c": "SFDX Package Distribution_Get Keys",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.GetPackageVersionWithKey\",\"parameters\":[{\"name\":\"subscriberVersionId\",\"value\":\"{$Context.JobExecution__r.DataJson.subscriberId}\"},{\"name\":\"dependenciesSubscriberIds\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\"}]}",
                    "copado__CustomType__c": "Flow",
                    "copado__JobTemplate__c": "a0u7Q0000004wGoQAI",
                    "copado__Order__c": 2,
                    "copado__Type__c": "Flow",
                    "Id": "a0t7Q000001EqBKQA0",
                    "Name": "Get Keys"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000000Mai4QAC"
                    },
                    "copado__ApiName__c": "SFDX Commit_Commit",
                    "copado__ConfigJson__c": "{\"functionName\":\"sfdx_commit\",\"parameters\":[{\"name\":\"file_changes_id\",\"value\":\"{$Context.JobExecution__r.DataJson.fileWithSelectedChanges}\",\"required\":true},{\"name\":\"file_name\",\"value\":\"Copado Commit changes.json\",\"required\":true},{\"name\":\"source_sessionid\",\"value\":\"{$Source.Credential.SessionId}\",\"required\":true},{\"name\":\"source_endpoint\",\"value\":\"{$Source.Credential.Endpoint}\",\"required\":true},{\"name\":\"namespace\",\"value\":\"\"},{\"name\":\"source_path\",\"value\":\"force-app\"},{\"name\":\"git_json\",\"value\":\"{$Context.Repository.Credential}\",\"required\":true},{\"name\":\"base_branch\",\"value\":\"{$Context.JobExecution__r.DataJson.baseBranch}\",\"required\":true},{\"name\":\"source_env\",\"value\":\"{$Source.apex.EnvironmentVariables}\",\"required\":true},{\"name\":\"find_and_replace_rules\",\"value\":\"{$Context.apex.GlobalFindAndReplaceSourceId}\",\"required\":false},{\"name\":\"feature_branch\",\"value\":\"{$Context.JobExecution__r.DataJson.featureBranchName}\",\"required\":true},{\"name\":\"recreate_if_exists\",\"value\":\"{$Context.JobExecution__r.DataJson.recreateFeatureBranch}\",\"required\":true},{\"name\":\"commit_message\",\"value\":\"{$Context.JobExecution__r.DataJson.message}\",\"required\":true},{\"name\":\"git_email\",\"value\":\"{$User.Email}\",\"required\":true},{\"name\":\"git_name\",\"value\":\"{$User.Name}\",\"required\":true},{\"name\":\"source_environment_branch\",\"value\":\"{$Context.apex.SourceEnvironmentBranch}\",\"required\":true},{\"name\":\"git_depth\",\"value\":\"100\"},{\"name\":\"timeout\",\"value\":\"180000\"},{\"name\":\"chunkSize\",\"value\":\"10\"}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q000000Xw28QAC",
                    "copado__Order__c": 1,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000000Mai4QAC",
                    "Name": "Commit"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000000Mai5QAC"
                    },
                    "copado__ApiName__c": "sfdx_deploy_sfdx_deploy",
                    "copado__ConfigJson__c": "{\"functionName\":\"sfdx_deploy\",\"parameters\":[{\"name\":\"file_changes_id\",\"value\":\"{$Context.JobExecution__r.DataJson.fileWithSelectedChanges}\",\"required\":true},{\"name\":\"promotion\",\"value\":\"{$Context.JobExecution__r.DataJson.promotionBranchName}\",\"required\":true},{\"name\":\"target_branch\",\"value\":\"{$Context.JobExecution__r.DataJson.destinationBranchName}\",\"required\":true},{\"name\":\"destination_instance_url\",\"value\":\"{$Destination.Credential.Endpoint}\",\"required\":true},{\"name\":\"destination_sessionid\",\"value\":\"{$Destination.Credential.SessionId}\",\"required\":true},{\"name\":\"git_json\",\"value\":\"{$Context.Repository.Credential}\",\"required\":true},{\"name\":\"destination_env\",\"value\":\"{$Destination.apex.EnvironmentVariables}\",\"required\":true},{\"name\":\"find_and_replace_rules\",\"value\":\"{$Context.apex.GlobalFindAndReplaceDestinationId}\"},{\"name\":\"conflict_resolution_attachments\",\"value\":\"{$Context.apex.GetConflictResolutionAttachments}\",\"required\":true},{\"name\":\"deploy_id\",\"value\":\"{$Job.ExecutionParent.Id}\",\"required\":true},{\"name\":\"isValidation\",\"value\":\"{$Context.JobExecution__r.DataJson.deploymentDryRun}\",\"required\":true},{\"name\":\"git_name\",\"value\":\"{$User.Name}\"},{\"name\":\"git_email\",\"value\":\"{$User.Email}\"},{\"name\":\"repository_id\",\"value\":\"{$Pipeline.Git_Repository__r.Id}\"},{\"name\":\"testLevel\",\"value\":\"{$Job.ExecutionParent.Promotion__r.cmcSf__Apex_Test_Level__c}\"},{\"name\":\"testOnlyClasses\",\"value\":\"{$Context.apex.cmcSf.GetTestOnlyClasses}\"},{\"name\":\"debugMode\",\"value\":\"{$Job.ExecutionParent.Promotion__r.cmcSf__Debug_Mode__c}\"},{\"name\":\"git_depth\",\"value\":\"100\"},{\"name\":\"validationId\",\"value\":\"{$Job.ExecutionParent.Promotion__r.cmcSf__Validate_Deploy_Request_Id__c}\"},{\"name\":\"testClasses\",\"value\":\"{$Context.apex.cmcSf.GetTestClasses}\"}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q000000Xw29QAC",
                    "copado__Order__c": 2,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000000Mai5QAC",
                    "Name": "Deploy"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v54.0/sobjects/copado__JobStep__c/a0t7Q000000M8RgQAK"
                    },
                    "copado__ApiName__c": "sfdx_deploy_Encode Deploy changes",
                    "copado__ConfigJson__c": "{\"functionName\":\"sfdx_encode_file_names\",\"parameters\":[{\"name\":\"file_changes_id\",\"value\":\"{$Context.JobExecution__r.DataJson.fileWithSelectedChanges}\"},{\"name\":\"file_name\",\"value\":\"Copado Deploy changes.json\"}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q000000Xw29QAC",
                    "copado__Order__c": 1,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000000M8RgQAK",
                    "Name": "Encode file names"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000000Mai7QAC"
                    },
                    "copado__ApiName__c": "sfdx_promote_sfdx_promote",
                    "copado__ConfigJson__c": "{\"functionName\":\"sfdx_promote\",\"parameters\":[{\"name\":\"user_stories\",\"value\":\"{$Context.JobExecution__r.DataJson.userStoryBranches}\",\"required\":true},{\"name\":\"promotion_branch\",\"value\":\"{$Context.JobExecution__r.DataJson.promotionBranchName}\",\"required\":true},{\"name\":\"target_branch\",\"value\":\"{$Context.JobExecution__r.DataJson.destinationBranchName}\",\"required\":true},{\"name\":\"tag\",\"value\":\"{$Context.JobExecution__r.Promotion__r.Release__r.Version__c}\",\"required\":false},{\"name\":\"git_json\",\"value\":\"{$Context.Repository.Credential}\",\"required\":true},{\"name\":\"recreatePromotionBranch\",\"value\":\"{$Context.JobExecution__r.Promotion__r.cmcSf__Recreate_Promotion_Branch__c}\",\"required\":false},{\"name\":\"promotion_id\",\"value\":\"{$Job.ExecutionParent.Id}\"},{\"name\":\"file_changes_id\",\"value\":\"{$Context.JobExecution__r.DataJson.fileWithSelectedChanges}\"},{\"name\":\"conflict_resolution_attachments\",\"value\":\"{$Context.apex.GetConflictResolutionAttachments}\"},{\"name\":\"git_name\",\"value\":\"{$User.Name}\"},{\"name\":\"git_email\",\"value\":\"{$User.Email}\"},{\"name\":\"repository_id\",\"value\":\"{$Pipeline.Git_Repository__r.Id}\"},{\"name\":\"git_depth\",\"value\":\"100\",\"required\":true}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q000000Xw2AQAS",
                    "copado__Order__c": 1,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000000Mai7QAC",
                    "Name": "Promote"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v52.0/sobjects/copado__JobStep__c/a0s09000000KT2TAAW"
                    },
                    "copado__ApiName__c": "SFDX Package Import_Retrieve Package Information",
                    "copado__ConfigJson__c": "{\"functionName\":\"Devhub_Package_Info\",\"parameters\":[{\"name\":\"PACKAGE\",\"value\":\"{$Context.JobExecution__r.DataJson.packageNameOrId}\"},{\"name\":\"SESSION\",\"value\":\"{$Source.Credential.SessionId}\"},{\"name\":\"BASEURL\",\"value\":\"{$Source.Credential.Endpoint}\"}]}",
                    "copado__JobTemplate__c": "a0t09000002vfccAAA",
                    "copado__Order__c": 2,
                    "copado__CustomType__c": "Function",
                    "copado__Type__c": "Function",
                    "CreatedDate": "2021-10-04T09:11:43.000+0000",
                    "Id": "a0s09000000KT2TAAW",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-04T09:59:20.000+0000",
                    "Name": "Retrieve Package Information",
                    "SystemModstamp": "2021-10-04T09:59:20.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobStep__c/a0s09000000KT2UAAW"
                    },
                    "copado__ApiName__c": "SFDX Package Import_Upsert Package Information",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.ImportPackageAndVersions\",\"parameters\":[{\"name\":\"packageJson\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\"},{\"name\":\"pipelineId\",\"value\":\"{$Context.JobExecution__r.DataJson.pipelineId}\"},{\"name\":\"jsonInformation\",\"value\":\"{$Context.JobExecution__r.DataJson.jsonInformation}\"},{\"name\":\"type\",\"value\":\"wait\"}]}",
                    "copado__JobTemplate__c": "a0t09000002vfccAAA",
                    "copado__Order__c": 3,
                    "copado__CustomType__c": "Flow",
                    "copado__Type__c": "Flow",
                    "CreatedDate": "2021-10-04T09:11:43.000+0000",
                    "Id": "a0s09000000KT2UAAW",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-31T21:44:48.000+0000",
                    "Name": "Upsert Package Information",
                    "SystemModstamp": "2021-10-31T21:44:48.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v52.0/sobjects/copado__JobStep__c/a0s09000000KT2WAAW"
                    },
                    "copado__ApiName__c": "SFDX Package Import_Update Source on Job Execution",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.Update_Source_on_Job_Execution\",\"parameters\":[{\"name\":\"type\",\"value\":\"wait\"},{\"name\":\"jsonInformation\",\"value\":\"{$Context.JobExecution__r.DataJson.jsonInformation}\"}]}",
                    "copado__JobTemplate__c": "a0t09000002vfccAAA",
                    "copado__Order__c": 1,
                    "copado__CustomType__c": "Flow",
                    "copado__Type__c": "Flow",
                    "CreatedDate": "2021-10-04T09:14:13.000+0000",
                    "Id": "a0s09000000KT2WAAW",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-04T10:48:24.000+0000",
                    "Name": "Update Source on Job Execution",
                    "SystemModstamp": "2021-10-04T10:48:24.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobStep__c/a0s09000000KT2VAAW"
                    },
                    "copado__ApiName__c": "SFDX Package Version Publish_Promote Package to Release",
                    "copado__ConfigJson__c": "{\"functionName\":\"sfdx_package_version_publish\",\"parameters\":[{\"name\":\"packageVersionId\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\",\"required\":true},{\"name\":\"SESSION\",\"value\":\"{$Destination.Credential.SessionId}\",\"required\":true},{\"name\":\"BASEURL\",\"value\":\"{$Destination.Credential.Endpoint}\",\"required\":true}]}",
                    "copado__JobTemplate__c": "a0t09000002vumlAAA",
                    "copado__Order__c": 2,
                    "copado__CustomType__c": "Function",
                    "copado__Type__c": "Function",
                    "CreatedDate": "2021-10-04T09:11:43.000+0000",
                    "Id": "a0s09000000KT2VAAW",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-25T00:06:07.000+0000",
                    "Name": "Publish Package Version",
                    "SystemModstamp": "2021-10-25T00:06:07.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobStep__c/a0s09000001cxfQAAQ"
                    },
                    "copado__ApiName__c": "SFDX Package Version Publish_Update Destination on Job Execution",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.UpdatePackageVersionRequisite\",\"parameters\":[{\"name\":\"packageVersionId\",\"value\":\"{$Context.JobExecution__r.DataJson.packageVersionId}\"},{\"name\":\"type\",\"value\":\"wait\"}]}",
                    "copado__JobTemplate__c": "a0t09000002vumlAAA",
                    "copado__Order__c": 1,
                    "copado__CustomType__c": "Flow",
                    "copado__Type__c": "Flow",
                    "CreatedDate": "2021-10-16T20:56:23.000+0000",
                    "Id": "a0s09000001cxfQAAQ",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-28T14:17:47.000+0000",
                    "Name": "Update Package Version Requisite",
                    "SystemModstamp": "2021-10-28T14:17:47.000+0000"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000001EqBUQA0"
                    },
                    "copado__ApiName__c": "SFDX Package Version Update_Update Package Version",
                    "copado__ConfigJson__c": "{\"functionName\":\"SFDX_Package_Version_Update\",\"parameters\":[{\"name\":\"SESSION\",\"value\":\"{$Destination.Credential.SessionId}\",\"required\":true},{\"name\":\"ENDPOINT\",\"value\":\"{$Destination.Credential.Endpoint}\",\"required\":true},{\"name\":\"packageVersion\",\"value\":\"{$Context.JobExecution__r.DataJson.packageVersion}\",\"required\":false},{\"name\":\"installationKey\",\"value\":\"{$Context.JobExecution__r.DataJson.installationKey}\"}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q0000004wGrQAI",
                    "copado__Order__c": 1,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000001EqBUQA0",
                    "Name": "Update Package Version"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000001EqBVQA0"
                    },
                    "copado__ApiName__c": "SFDX Package Version Update_Update Records",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.UpdatePackageVersion\",\"parameters\":[{\"name\":\"newVersion\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\"},{\"name\":\"type\",\"value\":\"wait\"}]}",
                    "copado__CustomType__c": "Flow",
                    "copado__JobTemplate__c": "a0u7Q0000004wGrQAI",
                    "copado__Order__c": 2,
                    "copado__Type__c": "Flow",
                    "Id": "a0t7Q000001EqBVQA0",
                    "Name": "Update Records"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v54.0/sobjects/copado__JobStep__c/a0s09000001eChjAAE"
                    },
                    "copado__ApiName__c": "SFDX Package Version Create_Create SFDX Package Version",
                    "copado__ConfigJson__c": "{\"functionName\":\"SFDX_Package_Version_Create\",\"parameters\":[{\"name\":\"ENDPOINT\",\"value\":\"{$Destination.Credential.Endpoint}\",\"required\":true},{\"name\":\"SESSION\",\"value\":\"{$Destination.Credential.SessionId}\",\"required\":true},{\"name\":\"packageId\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\",\"required\":false},{\"name\":\"versionName\",\"value\":\"{$Context.JobExecution__r.DataJson.versionName}\"},{\"name\":\"versionNumber\",\"value\":\"{$Context.JobExecution__r.DataJson.versionNumber}\"},{\"name\":\"description\",\"value\":\"{$Context.JobExecution__r.DataJson.description}\"},{\"name\":\"jsonInformation\",\"value\":\"{$Context.JobExecution__r.DataJson.jsonInformation}\",\"required\":false},{\"name\":\"git_json\",\"value\":\"{$Context.Repository.Credential}\",\"required\":true}]}",
                    "copado__JobTemplate__c": "a0t09000002vuuvAAA",
                    "copado__Order__c": 2,
                    "copado__CustomType__c": "Function",
                    "copado__Type__c": "Function",
                    "CreatedDate": "2022-01-19T08:36:42.000+0000",
                    "Id": "a0s09000001eChjAAE",
                    "IsDeleted": false,
                    "LastModifiedDate": "2022-02-26T10:46:30.000+0000",
                    "Name": "Create SFDX Package Version",
                    "SystemModstamp": "2022-02-26T10:46:30.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobStep__c/a0s09000001cxhHAAQ"
                    },
                    "copado__ApiName__c": "SFDX Package Version Create_Update Destination on Job Execution",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.UpdatePackageRequisite\",\"parameters\":[{\"name\":\"packageId\",\"value\":\"{$Context.JobExecution__r.DataJson.packageId}\"},{\"name\":\"type\",\"value\":\"wait\"}]}",
                    "copado__JobTemplate__c": "a0t09000002vuuvAAA",
                    "copado__Order__c": 1,
                    "copado__CustomType__c": "Flow",
                    "copado__Type__c": "Flow",
                    "CreatedDate": "2021-10-17T19:48:06.000+0000",
                    "Id": "a0s09000001cxhHAAQ",
                    "IsDeleted": false,
                    "LastModifiedDate": "2021-10-28T12:42:15.000+0000",
                    "Name": "Update Package Requisite",
                    "SystemModstamp": "2021-10-28T12:42:15.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobStep__c/a0s09000001eChlAAE"
                    },
                    "copado__ApiName__c": "SFDX Package Version Publish_Insert Package Version Information",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.CreatePackageVersion\",\"parameters\":[{\"name\":\"packageVersionJson\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\"},{\"name\":\"jsonInformation\",\"value\":\"{$Context.JobExecution__r.DataJson.jsonInformation}\"}]}",
                    "copado__JobTemplate__c": "a0t09000002vuuvAAA",
                    "copado__Order__c": 3,
                    "copado__CustomType__c": "Flow",
                    "copado__Type__c": "Flow",
                    "CreatedDate": "2022-01-19T08:36:42.000+0000",
                    "Id": "a0s09000001eChlAAE",
                    "IsDeleted": false,
                    "LastModifiedDate": "2022-01-31T08:36:36.000+0000",
                    "Name": "Insert Package Version Information",
                    "SystemModstamp": "2022-01-31T08:36:36.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v54.0/sobjects/copado__JobStep__c/a0s09000001eChmAAE"
                    },
                    "copado__ApiName__c": "SFDX Package Create_Create Package on Destination",
                    "copado__ConfigJson__c": "{\"functionName\":\"SFDXPackageCreate\",\"parameters\":[{\"name\":\"ENDPOINT\",\"value\":\"{$Destination.Credential.Endpoint}\",\"required\":true},{\"name\":\"SESSION\",\"value\":\"{$Destination.Credential.SessionId}\",\"required\":true},{\"name\":\"packageName\",\"value\":\"{$Job.ExecutionParent.Name}\",\"required\":false},{\"name\":\"path\",\"value\":\"{$Job.ExecutionParent.copado__DefaultPath__c}\"},{\"name\":\"jsonInformation\",\"value\":\"{$Context.JobExecution__r.DataJson.jsonInformation}\"},{\"name\":\"description\",\"value\":\"{$Job.ExecutionParent.copado__Description__c}\"},{\"name\":\"git_json\",\"value\":\"{$Context.Repository.Credential}\",\"required\":true},{\"name\":\"branch\",\"value\":\"{$Job.ExecutionParent.copado__DefaultBranch__c}\"}]}",
                    "copado__JobTemplate__c": "a0t09000002wVAsAAM",
                    "copado__Order__c": 2,
                    "copado__CustomType__c": "Function",
                    "copado__Type__c": "Function",
                    "CreatedDate": "2022-01-19T08:36:42.000+0000",
                    "Id": "a0s09000001eChmAAE",
                    "IsDeleted": false,
                    "LastModifiedDate": "2022-02-26T11:31:24.000+0000",
                    "Name": "Create Package on Destination",
                    "SystemModstamp": "2022-02-26T11:31:24.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobStep__c/a0s09000001d0rxAAA"
                    },
                    "copado__ApiName__c": "SFDX Package Create_Update Package",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.UpdatePackage\",\"parameters\":[{\"name\":\"packageId\",\"value\":\"{$Context.JobExecution__r.DataJson.packageId}\"},{\"name\":\"package2Id\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\"}]}",
                    "copado__JobTemplate__c": "a0t09000002wVAsAAM",
                    "copado__Order__c": 3,
                    "copado__CustomType__c": "Flow",
                    "copado__Type__c": "Flow",
                    "CreatedDate": "2021-11-23T11:50:09.000+0000",
                    "Id": "a0s09000001d0rxAAA",
                    "IsDeleted": false,
                    "LastModifiedDate": "2022-01-07T08:16:40.000+0000",
                    "Name": "Update Package",
                    "SystemModstamp": "2022-01-07T08:16:40.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v53.0/sobjects/copado__JobStep__c/a0s09000001diC2AAI"
                    },
                    "copado__ApiName__c": "SFDX Package Create_Update Destination on Job Ex",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.UpdateDestinationOnJobExecution\",\"parameters\":[{\"name\":\"packageId\",\"value\":\"{$Context.JobExecution__r.DataJson.packageId}\"}]}",
                    "copado__JobTemplate__c": "a0t09000002wVAsAAM",
                    "copado__Order__c": 1,
                    "copado__CustomType__c": "Flow",
                    "copado__Type__c": "Flow",
                    "CreatedDate": "2022-01-07T08:10:56.000+0000",
                    "Id": "a0s09000001diC2AAI",
                    "IsDeleted": false,
                    "LastModifiedDate": "2022-01-08T18:08:05.000+0000",
                    "Name": "Update Destination on Job Execution",
                    "SystemModstamp": "2022-01-08T18:08:05.000+0000"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v54.0/sobjects/copado__JobStep__c/a0t7Q000000Li7RQAS"
                    },
                    "copado__ApiName__c": "SFDX Run Apex Tests_Run Apex Tests",
                    "copado__ConfigJson__c": "{\"functionName\":\"SFDXRunApexTests\",\"parameters\":[{\"name\":\"source_session_id\",\"value\":\"{$Source.Credential.SessionId}\"},{\"name\":\"source_endpoint\",\"value\":\"{$Source.Credential.Endpoint}\"},{\"name\":\"namespace\",\"value\":\"\"},{\"name\":\"source_path\",\"value\":\"force-app\"},{\"name\":\"test_minutes_timeout\",\"value\":\"1440\"},{\"name\":\"test_result_ids\",\"value\":\"{$Context.JobExecution__r.DataJson.resultIds}\"},{\"name\":\"test_classes\",\"value\":\"{$Context.apex.cmcSf.GetTestClasses}\"},{\"name\":\"consolidated_result_id\",\"value\":\"{$Context.JobExecution__r.DataJson.resultId}\"}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q000000XrrkQAC",
                    "copado__Order__c": 3,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000000Li7RQAS",
                    "Name": "Run Apex Tests"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v54.0/sobjects/copado__JobStep__c/a0t7Q000000Li7SQAS"
                    },
                    "copado__ApiName__c": "SFDX Run Apex Tests_Populate Source Environment",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.SetSourceEnvironmentOnApexTestJobExecution\",\"parameters\":[{\"name\":\"jobExecutionId\",\"value\":\"{$Context.JobExecution__r.Id}\"},{\"name\":\"testIds\",\"value\":\"{$Context.JobExecution__r.DataJson.testIds}\"}]}",
                    "copado__CustomType__c": "Salesforce Flow",
                    "copado__JobTemplate__c": "a0u7Q000000XrrkQAC",
                    "copado__Order__c": 1,
                    "copado__Type__c": "Flow",
                    "Id": "a0t7Q000000Li7SQAS",
                    "Name": "Set Source Environment"
                },
                {
                    "attributes": {
                        "type": "copado__JobStep__c",
                        "url": "/services/data/v54.0/sobjects/copado__JobStep__c/a0t7Q000000Li7TQAS"
                    },
                    "copado__ApiName__c": "SFDX Run Apex Tests_Evaluate Apex Tests Acceptance Criteria",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.EvaluateApexTestsAcceptanceCriteria\",\"parameters\":[{\"name\":\"acceptanceCriteria\",\"value\":\"{$Context.JobExecution__r.DataJson.acceptanceCriteria}\"},{\"name\":\"resultIds\",\"value\":\"{$Context.JobExecution__r.DataJson.resultIds}\"},{\"name\":\"consolidatedResultId\",\"value\":\"{$Context.JobExecution__r.DataJson.resultId}\"}]}",
                    "copado__CustomType__c": "Salesforce Flow",
                    "copado__JobTemplate__c": "a0u7Q000000XrrkQAC",
                    "copado__Order__c": 4,
                    "copado__Type__c": "Flow",
                    "Id": "a0t7Q000000Li7TQAS",
                    "Name": "Evaluate Apex Tests Acceptance Criteria"
                },
                {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000001AJ2fQAG"
                    },
                    "copado__ApiName__c": "SFDX Package Version Git Configure_Git Config",
                    "copado__ConfigJson__c": "{\"functionName\":\"SFDX_Package_Version_Git_Config\",\"parameters\":[{\"name\":\"packageVersion\",\"value\":\"{$Context.JobExecution__r.DataJson.packageVersion}\",\"required\":false},{\"name\":\"git_json\",\"value\":\"{$Context.Repository.Credential}\",\"required\":false}]}",
                    "copado__CustomType__c": "Function",
                    "copado__JobTemplate__c": "a0u7Q0000004yiqQAA",
                    "copado__Order__c": 1,
                    "copado__Type__c": "Function",
                    "Id": "a0t7Q000001AJ2fQAG",
                    "Name": "Git Config"
                  },
                  {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0t7Q000001AJ2gQAG"
                    },
                    "copado__ApiName__c": "SFDX Package Version Git Configure_1_Update Record",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.UpdatePackageVersion\",\"parameters\":[{\"name\":\"newVersion\",\"value\":\"{$Job.PrevStep.Result__r.Result_Data__c}\"},{\"name\":\"type\",\"value\":\"wait\"}]}",
                    "copado__CustomType__c": "Salesforce Flow",
                    "copado__JobTemplate__c": "a0u7Q0000004yiqQAA",
                    "copado__Order__c": 2,
                    "copado__Type__c": "Flow",
                    "Id": "a0t7Q000001AJ2gQAG",
                    "Name": "Update Record"
                  },
                  {
                    "attributes": {
                      "type": "copado__JobStep__c",
                      "url": "/services/data/v55.0/sobjects/copado__JobStep__c/a0s090000020VnJAAU"
                    },
                    "copado__ApiName__c": "SFDX Test Records Creation In User Story_1_Apex Test Record Automation_1",
                    "copado__ConfigJson__c": "{\"flowName\":\"cmcSf.Apex_Test_Record_Automation\",\"parameters\":[{\"name\":\"userStoryId\",\"value\":\"{$Context.JobExecution__r.DataJson.userStoryId}\"},{\"name\":\"recreateFeatureBranch\",\"value\":\"{$Context.JobExecution__r.DataJson.recreateFeatureBranch}\"}]}",
                    "copado__CustomType__c": "Salesforce Flow",
                    "copado__JobTemplate__c": "a0t09000000lkYtAAI",
                    "copado__Order__c": 1,
                    "copado__Type__c": "Flow",
                    "Id": "a0s090000020VnJAAU",
                    "Name": "Apex Test Record Automation"
                  }
            ],
            "ObjectType": "copado__JobStep__c"
        }
    ],
    "blobsByUID": {}
}
