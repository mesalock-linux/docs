# Run Mesalock Linux with SEV Kata container

## Setup SEV Kata environment with ubuntu 18.04
Follow the AMD's guide to setup SEV environment
https://github.com/AMDESE/AMDSEV/tree/kata#ubuntu18-kata-install

## Launch Mesalock Linux with SEV Kata container
``` bash
sudo docker run -it mesalocklinux/mesalock-linux
```

## Check if Mealock linux is running in SEV Kata container
* Download qemu QMP
    ``` bash
    git clone https://github.com/qemu/qemu.git
    cd qemu/scripts/qmp/
    ```
* Get qemu QMP socket path
    ``` bash
    ps aux | grep qemu
    # output example:
    # /usr/local/bin/qemu-system-x86_64 ... -qmp unix:/run/vc/sbs/c083174939296163e3d87fc22201fc8fd0ddac23e37849cf330cac48ade6502c/qmp.sock ...
    ```
    *``/run/vc/sbs/c083174939296163e3d87fc22201fc8fd0ddac23e37849cf330cac48ade6502c/qmp.sock``* is the socket path you need.

* Run qemu QMP command ```query-sev-capabilities``` 
    ``` bash
    sudo ./qmp-shell socket_path

    (QEMU) query-sev-capabilities
    ```
    Then you will see host PDH, cert-chain, cbitpos and reduced-phys-bits

* Try to dump container's physical memory
    * Write a script in mesalock linux
    ```bash
    echo -e "let key1=\"pass1\" \n \
    let key2=\"secret\" \n \
    while true \n \
    sleep 1s \n \
    let c=\$key1\$key2 \n \
    end" > script.sh
    ```
    * Run the script ```script.sh```. "pass1secret" is in memory now
    
    * Dump physical memory with qemu QMP
    ``` bash
    (QMP) dump-guest-memory paging=true protocol=file:/tmp/dumpmem
    ```

    * check dumped memory
    ```
    # output: Binary file /tmp/dumpmem matches
    sudo grep pass1 /tmp/dumpmem

    # no matched content
    sudo grep pass1secret /tmp/dumpmem
    ```