cd /etc/systemd/system

nano file_name.socket


[Unit]
Description=file_name socket

[Socket]
ListenStream=/run/file_name.sock

[Install]
WantedBy=sockets.target


nano file_name.service

[Unit]
Description=file_name daemon
Requires=file_name.socket
After=network.target

[Service]
User=pushpan
Group=www-data
WorkingDirectory=/home/pushpan/project_dir
ExecStart=/home/pushpan/miniconda3/envs/env_name/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/file_name.sock \
          core.wsgi:application

[Install]
WantedBy=default.target


sudo systemctl start file_name.socket
sudo systemctl enable file_name.socket

sudo systemctl status file_name.socket

file /run/file_name.sock

sudo systemctl daemon-reload

sudo systemctl restart file_name

sudo systemctl status file_name

curl --unix-socket /run/file_name.sock localhost
