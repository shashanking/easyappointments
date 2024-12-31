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

## Production Deployment

### Prerequisites for Production
- A domain name pointing to your server
- SSH access to your server
- Docker and Docker Compose installed on the server

### Production Setup Steps

1. Clone the repository on your production server
2. Copy `.env.example` to `.env` and configure your environment variables:
```bash
cp .env.example .env
nano .env
```

3. Create required directories:
```bash
mkdir -p docker/mysql docker/mysql-backup certbot/conf certbot/www uploads config
```

4. Start the production stack:
```bash
docker-compose -f docker-compose.prod.yml up -d
```

### SSL Certificate Setup

The production setup includes automatic SSL certificate generation and renewal through Let's Encrypt. On first deployment:

1. Comment out the SSL parts in nginx.conf
2. Start the stack to get initial certificates:
```bash
docker-compose -f docker-compose.prod.yml up -d
```
3. Generate initial certificate:
```bash
docker-compose -f docker-compose.prod.yml run --rm certbot certonly --webroot -w /var/www/certbot -d yourdomain.com
```
4. Uncomment SSL parts in nginx.conf and restart nginx:
```bash
docker-compose -f docker-compose.prod.yml restart nginx
```

## Upgrade Procedures

### Minor Version Updates

1. Pull the latest images:
```bash
docker-compose -f docker-compose.prod.yml pull
```

2. Restart the services:
```bash
docker-compose -f docker-compose.prod.yml up -d
```

### Major Version Updates

1. Backup your database:
```bash
docker-compose -f docker-compose.prod.yml exec mysql mysqldump -u root -p easyappointments > backup.sql
```

2. Update the image version in docker-compose.prod.yml
3. Pull and restart:
```bash
docker-compose -f docker-compose.prod.yml pull
docker-compose -f docker-compose.prod.yml up -d
```

## Backup and Restore

### Database Backup
```bash
# Manual backup
docker-compose -f docker-compose.prod.yml exec mysql mysqldump -u root -p easyappointments > backup.sql

# Automated daily backups are stored in docker/mysql-backup
```

### Database Restore
```bash
# Stop the stack
docker-compose -f docker-compose.prod.yml down

# Restore from backup
docker-compose -f docker-compose.prod.yml up -d mysql
cat backup.sql | docker exec -i $(docker-compose -f docker-compose.prod.yml ps -q mysql) mysql -u root -p easyappointments

# Start the stack
docker-compose -f docker-compose.prod.yml up -d
```

## Monitoring and Maintenance

### View Logs
```bash
# All services
docker-compose -f docker-compose.prod.yml logs -f

# Specific service
docker-compose -f docker-compose.prod.yml logs -f easyappointments
```

### Check Service Status
```bash
docker-compose -f docker-compose.prod.yml ps
```

### Resource Usage
```bash
docker stats
```

## Security Considerations

1. Change all default passwords in production
2. Enable HTTPS (included in production setup)
3. Regular security updates:
```bash
docker-compose -f docker-compose.prod.yml pull
docker-compose -f docker-compose.prod.yml up -d
```
4. Regular backups (automated in production setup)
5. Monitor logs for suspicious activity
6. Use strong passwords for all services
7. Configure firewall rules on your server

## License

This setup is provided under the same license as Easy!Appointments. For more information, visit [Easy!Appointments GitHub Repository](https://github.com/alextselegidis/easyappointments).
