# home
my_work_repo
#!/bin/bash
 
# List of servers
servers=("ukpe01vr" "ukpe02vr" "ukpe03vr" "ukpe04vr")
servers2=("ukpe01vr" "ukpe02vr")
 
# Ask for release number
echo -n "Enter the release number: "
read release_number
 
# Move directory 'JEE' to 'JEE_B4_release_number'
for server in "${servers[@]}"
do
    ssh -q petcrm1@$server "mv JEE JEE_B4_$release_number"
    echo "Backup of 'JEE' on $server completed."
done

 # Move directory 'JEE' to 'JEE_B4_release_number'
for server_2 in "${servers2[@]}"
do
    ssh -q petoms1@$server_2 "mv JEE JEE_B4_$release_number"
    echo "Backup of 'JEE' on $server completed."
done

# Confirm backup
echo "Confirming backups..."
for server in "${servers[@]}"
do
    ssh -q petcrm1@$server "[ -d JEE_B4_$release_number ] && echo 'Backup on $server confirmed.' || echo 'Backup on $server failed.'"
done
 
 # Confirm backup
echo "Confirming backups..."
for server_2 in "${servers2[@]}"
do
    ssh -q petoms1@$server_2 "[ -d JEE_B4_$release_number ] && echo 'Backup on $server_2 confirmed.' || echo 'Backup on $server_2 failed.'"
done
 
# Move 'JEE_releasenumber' to 'JEE'

 
for server in "${servers[@]}"
do
    ssh -q petcrm1@$server "mv JEE_$release_number JEE"
    echo "Restoration of 'JEE' on $server completed."
done
 
# Confirm copy
echo "Confirming restoration..."
for server in "${servers[@]}"
do
    ssh -q petcrm1@$server "[ -d JEE ] && echo 'Restoration on $server confirmed.' || echo 'Restoration on $server failed.'"
done
 
 for server_2 in "${servers2[@]}"
do
    ssh -q petoms1@$server_2 "mv JEE_$release_number JEE"
    echo "Restoration of 'JEE' on $server_2 completed."
done
 
# Confirm copy
echo "Confirming restoration..."
for server_2 in "${servers2[@]}"
do
    ssh -q petoms1@$server_2 "[ -d JEE ] && echo 'Restoration on $server_2 confirmed.' || echo 'Restoration on $server_2 failed.'"
done
# Get build number from a specific file
##build_number=$(ssh petcrm1@$server "cat /vfkuser1/pet/petcrm1/JEE/CRMProduct/application/crm_build.number")
##build_number_2=$(ssh petoms1@$server_2 "cat /vfkuser2/pet/petoms1/JEE/config/oms/oms_build.number")
 
##echo "Build number of CRM is: $build_number"
##echo "Build number of OMS is: $build_number_2"

#get the build number##
buildpath=/petnas/vfk/genadm/Build_confirm_dir/CRM_BUILD
buildpath_2=/petnas/vfk/genadm/Build_confirm_dir/OMS_BUILD
for server in "${servers[@]}"
do
    ssh -q petcrm1@$server "cat /vfkuser1/pet/petcrm1/JEE/CRMProduct/application/crm_build.number >>$buildpath/CRM_BUILD.NUMBER"
    echo "the build number fetched from all the $server completed."
done

for server_2 in "${servers2[@]}"
do
    ssh -q petoms1@$server_2 "cat /vfkuser2/pet/petoms1/JEE/config/oms/oms_build.number >>$buildpath_2/OMS_BUILD.NUMBER"
    echo "the build number fetched from all the $server_2 completed.."
done

Build_output=/petnas/vfk/genadm/Build_confirm_dir
CRM_BUILD_NUM=(cat $Build_output/CRM_BUILD)
OMS_BUILD_NUM=(cat $Build_output/OMS_BUILD)
echo "the build number of the CRM is :$CRM_BUILD_NUM"
echo "the build number of the OMS is :$OMS_BUILD_NUM"
