### Subdomain Enumeration

```bash
echo -n "Enter the target domain : "; read target; subdomain="${target}"; output="domains.txt"; subfinder -d "$subdomain" -o "subfinder_${subdomain}.txt" && echo "subfinder: $(wc -l < subfinder_${subdomain}.txt)"; cat "subfinder_${subdomain}.txt" >> "$output"; assetfinder --subs-only "$subdomain" > d.txt && echo "assetfinder: $(wc -l < d.txt)"; cat d.txt >> "$output"; findomain --target "$subdomain" --unique-output "findomain_${subdomain}.txt" && echo "findomain: $(wc -l < findomain_${subdomain}.txt)"; cat "findomain_${subdomain}.txt" >> "$output"; sublist3r -d "$subdomain" -o "sublist3r_${subdomain}.txt" && echo "sublist3r: $(wc -l < sublist3r_${subdomain}.txt)"; cat "sublist3r_${subdomain}.txt" >> "$output"; subdominator -d "$subdomain" -o "subdominator_${subdomain}.txt" && echo "subdominator: $(wc -l < subdominator_${subdomain}.txt)"; cat "subdominator_${subdomain}.txt" >> "$output"; sort -u "$output" -o "$output"; echo "Subdomain enumeration complete. Results saved in $output."; echo "Total unique subdomains in $output: $(wc -l < $output)"; rm -f subfinder_${subdomain}.txt d.txt findomain_${subdomain}.txt sublist3r_${subdomain}.txt subdominator_${subdomain}.txt
```


### Find live Host

```bash
httpx -l domains.txt -o live-urls.txt
```


### Resolved host to ip's

```bash
cat live-urls.txt | sed -E 's|https?://||' | while read domain; do dig +short "$domain" | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | tail -n1; done > resolved-ips.txt
```


### Mass scan to find open port fastly

```bash
sudo masscan -p1-65535 --rate=3000 -iL resolved-ips.txt | awk '/Discovered open port/ {print "[URL : "$6"] [OPEN PORTS]\n"$4}' > ports.txt
```


### Get clean target mean ip with port

```bash
awk '/\[URL/ {ip=$3} /^[0-9]/ {gsub(/\[|\]/, "", ip); print ip":"$1}' ports.txt | sed 's#/tcp##g' > targets.txt
```


### Now run NMAP Here

```bash
while IFS=: read -r ip port; do echo -e "\e[1;32mScanning $ip on port $port...\e[0m" | tee -a service-enum.txt; sudo nmap -sV -Pn -T4 -p"$port" "$ip" | tee -a service-enum.txt; done < targets.txt
```
