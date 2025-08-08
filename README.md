# PfSense OpenVPN Exporter

Simple PHP endpoint to obtain OpenVPN service metrics on a pfSense Firewall for use with Prometheus.

## How to deploy PHP Endpoint on pfSense server
1. Install Filer package to your pFsense

2. Create a file in **Diagnostics > Filer > Files > add**
   Edit fields:
   **File:** "/usr/local/www/pfsense_openvpn_exporter.php"

   **Description:** "openVPN metrics exporter"

   **Permissions:** left blank or 622
   
   **File contents:** place php code here
   
   under **Command to run after file save/sync.** edit:
   
   **Script/Command:** nginx -s reload
   
   **Execute mode:** Background (default)
   
4. Press **Save**
   
5. Go to your pfsense_ip_server:pfsense_port/pfsense_openvpn_exporter.php  

## Example of Use

Once deployed, you can configure Prometheus to scrape metrics from the PHP endpoint. Add the following job to your Prometheus configuration:
```yaml
scrape_configs:
  - job_name: 'pfsense_openvpn'
    static_configs:
      - targets: ['<pfsense_ip_server:pfsense_port>']
        labels:
          instance: 'pfsense_openvpn'
        metrics_path: '/pfsense_openvpn_exporter.php'
```
Replace <pfsense_ip_server> with the IP address of your pfSense server and pfsense_port with your console managment port

Add a [dashboard](https://github.com/iligl/pfsense_openvpn_exporter/blob/main/grafana_pfsense_vpn.json) to your grafana. 

## Troubleshooting

If you encounter issues, ensure that:

    The pfsense_openvpn_exporter.php file has been correctly copied to the /usr/local/www directory.
    NGINX is running and serving files from the /usr/local/www directory.


## Acknowledgments
Forked from [vielca project](https://github.com/vielca/pfsense_openvpn_exporter), dashbord added
