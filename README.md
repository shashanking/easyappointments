# Easy!Appointments Docker Setup

This repository contains a Docker setup for running Easy!Appointments, an open-source appointment scheduling system.

## Prerequisites

- Docker
- Docker Compose

## Quick Start

1. Clone this repository:
```bash
git clone <your-repository-url>
cd <repository-directory>
```

2. Start the application:
```bash
docker-compose up -d
```

3. Access the application:
Open your web browser and navigate to `http://localhost:8080`

## Configuration

The setup includes:
- Easy!Appointments application (latest version)
- MySQL 8.0 database

### Environment Variables

#### Easy!Appointments Service:
- `BASE_URL`: http://localhost:8080
- `DEBUG_MODE`: TRUE
- `DB_HOST`: mysql
- `DB_NAME`: easyappointments
- `DB_USERNAME`: root
- `DB_PASSWORD`: secret

#### MySQL Service:
- `MYSQL_ROOT_PASSWORD`: secret
- `MYSQL_DATABASE`: easyappointments

## Data Persistence

MySQL data is persisted in the `./docker/mysql` directory.

## Ports

- Easy!Appointments: Port 8080
- MySQL: Internal only (not exposed)

## Stopping the Application

To stop the application:
```bash
docker-compose down
```

## Troubleshooting

1. If you can't connect to the application:
   - Ensure Docker is running
   - Check if the ports are available
   - View logs: `docker-compose logs`

2. For database issues:
   - Check MySQL logs: `docker-compose logs mysql`
   - Ensure the database volume permissions are correct

## Security Note

The default credentials in this setup are for development purposes only. In a production environment, you should:
- Change all default passwords
- Use environment variables for sensitive data
- Enable HTTPS
- Follow security best practices

## License

This setup is provided under the same license as Easy!Appointments. For more information, visit [Easy!Appointments GitHub Repository](https://github.com/alextselegidis/easyappointments).
