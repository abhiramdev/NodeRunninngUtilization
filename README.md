# NodeRunninngUtilization
#To find the utilization of storage 
#Copy this to a file and name it as NodeRunninngUtilization.sh
rm NodeRunninngUtilization.sh && nano NodeRunninngUtilization.sh
## Copy the followinng code : 

echo "Docker containers sorted by size (descending order):"
echo "---------------------------------------------------"

docker ps -as --format "{{.Size}}\t{{.Names}}\t{{.ID}}" | sort -h -r | \
while read size name id; do
    echo -e "$size\t$name\t$id"
done

echo -e "\nContainer with maximum storage usage:"
echo "-----------------------------------------"

docker ps -as --format "{{.Size}}\t{{.Names}}\t{{.ID}}" | sort -h -r | head -n1

echo -e "\nDetailed storage information for the largest container:"
echo "-----------------------------------------------------------"

largest_container=$(docker ps -as --format "{{.ID}}" | head -n1)
docker inspect -s $largest_container | jq '.[0].SizeRootFs, .[0].SizeRw'

## Hit CTRL+ O and Hit yes 
## CTRL + X

#If JQ not installed : 

sudo apt-get install jq

#Run

bash NodeRunninngUtilization.sh
