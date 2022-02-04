---
name: Deployment with Systemctl 
sort: 2
---

# Systemctl

Systemctl command is a command used to manage and control services. It allows you to enable, disable, view, start, stop, or restart system services.
 
## Install radiant application as a service

1. Pack your application using `radical pack` command. Copy the resultant `.tar.gz` file to target server.
		
2. Unpack 
                
		mkdir -p /usr/local/radicalpkg && cd "$_"
		tar -xvzf *.tar.gz
		rm -rf *.tar.gz
		
3. Configure service parameters

		cat <<'EOF' > /etc/systemd/system/radicalpkg.service
        [Unit]
        Description=radicalpkg
        AssertPathExists=/usr/local/radicalpkg
        
        [Service]
        WorkingDirectory=/usr/local/radicalpkg
        ExecStart=/usr/local/radicalpkg/radicalpkg
        
        ExecReload=/bin/kill -HUP $MAINPID
        LimitNOFILE=65536
        Restart=always
        RestartSec=5
        
        [Install]
        WantedBy=multi-user.target
        EOF

3. Install service 

        chmod +x radicalpkg
        chmod 644 /etc/systemd/system/radicalpkg.service
        systemctl daemon-reload
        systemctl enable radicalpkg
        systemctl start radicalpkg
        systemctl status radicalpkg

		
		



