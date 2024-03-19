import os
import yaml

def check_directories_in_docker_compose(file_path):
    # Load the docker-compose file
    with open(file_path, 'r') as file:
        docker_compose = yaml.safe_load(file)

    directories_to_create = []

    # Parse the docker-compose file
    for service_name, service in docker_compose['services'].items():
        volumes = service.get('volumes', [])
        for volume in volumes:
            host_path = volume.split(':')[0]
            if not os.path.isabs(host_path):
                print(f"Skipping relative volume {host_path} in service {service_name}")
                continue
            if not os.path.exists(host_path):
                directories_to_create.append(host_path)

    # Remove duplicates
    directories_to_create = list(set(directories_to_create))

    if directories_to_create:
        print("The following directories need to be created:")
        for directory in directories_to_create:
            print(directory)
        consent = input("Do you want to create these directories? (y/n): ")
        if consent.lower() == 'y':
            for directory in directories_to_create:
                try:
                    os.makedirs(directory)
                    print(f"Created directory: {directory}")
                except OSError as error:
                    print(f"Error creating directory {directory}: {error}")
        else:
            print("Operation cancelled by user.")
    else:
        print("All directories already exist.")

if __name__ == "__main__":
    file_path = input("Enter the path to your docker-compose file: ")
    check_directories_in_docker_compose(file_path)
