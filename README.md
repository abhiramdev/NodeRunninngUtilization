# NodeRunninngUtilization
#To find the utilization of storage 
#Copy this to a file and name it as NodeRunninngUtilization.sh
rm NodeRunninngUtilization.sh && nano NodeRunninngUtilization.sh
## Copy the followinng code : 

#!/bin/bash

echo "Docker containers sorted by size (largest first):"
echo "------------------------------------------------"

docker ps -as --format "{{.Size}}\t{{.Names}}\t{{.ID}}" | sort -hr | while read size name id
do
    # Extract the numeric part of the size
    size_num=$(echo $size | sed 's/[^0-9.]*//g')
    
    # Extract the unit (B, kB, MB, GB)
    unit=$(echo $size | sed 's/[0-9.]*//g' | sed 's/^(//' | sed 's/)$//')
    
    # Convert to GB
    case $unit in
        B)  size_gb=$(echo "$size_num / 1024 / 1024 / 1024" | awk '{printf "%.6f", $1}') ;;
        kB) size_gb=$(echo "$size_num / 1024 / 1024" | awk '{printf "%.6f", $1}') ;;
        MB) size_gb=$(echo "$size_num / 1024" | awk '{printf "%.6f", $1}') ;;
        GB) size_gb=$size_num ;;
        *)  size_gb="0" ;;
    esac
    
    printf "%-15.2f %-20s %-20s\n" $size_gb "$name" "$id"
done

echo -e "\nNote: Sizes are in GB and represent the virtual size of the container."

## Hit CTRL+ O and Hit yes 
## CTRL + X

#If JQ not installed : 

sudo apt-get install jq

#Run

bash NodeRunninngUtilization.sh
