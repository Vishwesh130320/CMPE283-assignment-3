# CMPE 283 Assignment-2
### Instrumentation Via Hyper-call (Add New CPUID Emulation Features in KVM)

- Our task is to change the CPUID emulation code in KVM such that it returns more information when a particular CPUID "leaf function" is invoked.

- For CPUID leaf node %eax=0x4FFFFFFC:

  - We have to return total number of exits (all catagories) in %eax

- For CPUID leaf node %eax=0x4FFFFFFD:
  - We have to return the top 32 bits of the total time spent processing all exits in%ebx.
  - The low 32 bits of the total time spent processing all exits in%ecx will be returned. \* Return values for %ebx and %ecx are measured in processor cycles across all VCPUs.
    At a high level, you will need to:
- Modify the kernel code with the assignment's functionality:
  - Decide where to put the measurement code.
  - Create new CPUID leaf 0x4FFFFFFD, 0x4FFFFFFC
- Make a user-mode application that executes the necessary CPUID instructions needed to test.
  - This is possible on Ubuntu by installing the 'cpuid' package.
  - Run this user mode program in the nested VM
- Check for correct output.

### Steps to reproduce

1. We followed the steps to install the VM, then Ubuntu on our Windows / Mac PC. Installed ISO - Ubuntu 18.5, allocated disk space of 250 GB.
![image](https://user-images.githubusercontent.com/99626312/205785339-2ae5c9f3-de40-4898-89f9-fe4e848db443.png)
![image](https://user-images.githubusercontent.com/99626312/205785357-f2f1a85f-9730-4ee8-9906-7e4c1c1b335c.png)


2. We have cloned the Linux github repository, using following command;
   `git clone https://github.com/Vishwesh130320/linux`
   ![image](https://user-images.githubusercontent.com/99626312/205785442-6ebf8077-27b0-4f24-84d1-bc7df5ba1d43.png)
   
3. `Install gcc make`

4. Installed the dependency;
   `sudo apt-get install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf`
![image](https://user-images.githubusercontent.com/99626312/205785661-c5336345-11f9-47eb-bf60-6047828913eb.png)

5. Checked the Linux Version (old):
   `uname -a`

6. `sudo make oldconfig` (and then just use the default for everything).
![image](https://user-images.githubusercontent.com/99626312/205785866-9e3cf9fd-7207-482e-9c74-6ac9204ddf07.png)

7. `cd linux`

8. following instruction in "Linux" folder: `make -j 8 modules && make -j 8 && sudo make modules_install && sudo make install` (This might take some time).

9. `sudo reboot`

- `uname -a`

10. We will make changes to cpuid.c at ~/linux/arch/X86/kvm
![image](https://user-images.githubusercontent.com/99626312/205786096-83e25b4e-6ecb-4043-b304-034f7c14ce1b.png)

11. we will make changes to vmx.c
![image](https://user-images.githubusercontent.com/99626312/205786172-ffbf9d36-68d6-44ab-9f1c-46efc2ed39d4.png)
### Testing
12. `sudo bash` and `make -j 8 modules && make -j 8 && sudo make modules_install && sudo make install`
![image](https://user-images.githubusercontent.com/99626312/205786285-e5ddead3-e989-4a37-905c-325a1c815e88.png)


13. `sudo apt install`
![image](https://user-images.githubusercontent.com/99626312/205787924-15370bd0-258e-441f-ba78-3d21f32b5056.png)

14. `sudo apt update`
![image](https://user-images.githubusercontent.com/99626312/205787988-2367ea55-df99-47b1-a935-40eff38b477b.png)

15. `sudo apt install --assume-yes wget tasksel`
![image](https://user-images.githubusercontent.com/99626312/205786386-30af60d7-5593-4d62-8fe5-3ab2fc66cd3b.png)


16. `wget https://dl.google.com/linux/direct/chrome-remote-desktop_current_amd64.deb`
![image](https://user-images.githubusercontent.com/99626312/205786498-e98eec58-8cb6-42f9-abf4-243bd1f3e448.png)


17. `sudo apt-get install --assume-yes ./chrome-remote-desktop_current_amd64.deb`
![image](https://user-images.githubusercontent.com/99626312/205788057-8a7afff4-0720-4107-bd04-c551f4621f52.png)

18. `sudo tasksel install ubuntu-desktop.`

19. `sudo bash -c ‘echo “exec /etc/X11/Xsession /usr/bin/gnome-session” > /etc/chrome-remote-desktop-session’`

20. `sudo apt-get install xbase-clients`
![image](https://user-images.githubusercontent.com/99626312/205788915-a15f92d0-323d-4c05-9058-62aaaf860072.png)

21. `sudo systemctl status chrome-remote-desktop@$USER`

22. `sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils`

23. `sudo adduser 'vishweshkrupeshbhai_shah' libvirt`

24. `sudo getent group | grep libvirt`

25. `sudo adduser 'vishweshkrupeshbhai_shah' kvm`

26. `sudo getent group | grep kvm`

27. `sudo virsh list --all`

28. `sudo systemctl enable --now libvirtd`

31. Setup remote desktop for debian Linux.
![image](https://user-images.githubusercontent.com/99626312/205789366-bf091268-01b8-4b82-a646-9d685a618bb7.png)

32. `sudo apt install virt-manager`
![image](https://user-images.githubusercontent.com/99626312/205789422-1ab9be88-fe5b-4b94-b511-4be73a1f2d1f.png)


33. Download ISO: `wget https://releases.ubuntu.com/ jammy/ubuntu-22.04.1-desktop-amd64.iso`
![image](https://user-images.githubusercontent.com/99626312/205789504-9b571d48-26c3-41ac-953c-051a985a9f50.png)

34. `sudo mv~/ubuntu-22.04.1-desktop-amd64.iso/var/lib/libvirt/images/`
![image](https://user-images.githubusercontent.com/99626312/205789611-114a4459-89e5-4c3b-a23a-86d551849b3c.png)

35. Now we would be able to see our instance on remote desktop.
![image](https://user-images.githubusercontent.com/99626312/205789828-70b3f445-191c-4188-b9a0-31c821e3e46d.png)

![image](https://user-images.githubusercontent.com/99626312/205789672-69221d1f-36ba-44c8-98cc-ac94a62c74df.png)

36. Now setup our nested virtual machine.

37. Install Ubantu.
![image](https://user-images.githubusercontent.com/99626312/205789869-998dafa4-b4ea-40f8-8fc0-a81bd144adbe.png)

38. Open terminal in ubantu and run commands:

39. `sudo apt update`


40. `sudo apt-get update`
![image](https://user-images.githubusercontent.com/99626312/205789934-39e747e5-b81a-4754-9e8c-9e39d8034ee5.png)

41. `sudo apt-get install build-essential`
![image](https://user-images.githubusercontent.com/99626312/205789991-f4635a1d-e355-431c-be01-ac0e0ed55e18.png)

42. `sudo apt-get install cpuid`
![image](https://user-images.githubusercontent.com/99626312/205790042-9184cc94-c948-42ed-a60d-56214e65e700.png)

### OutPut
- Referring to the passages pasted above, the frequency of exits continued to rise steadily but not necessarily linearly (exits for exit 1). After restarting the inner VM, I noticed a significant increase in exit numbers. Approximately 3973521 exits happened following a complete VM boot.

- Below were the most frequent exits:

<img width="338" alt="166074306-30118d48-f0e7-4746-b928-64e963091115" src="https://user-images.githubusercontent.com/99626312/207219586-ff566296-1e9c-4053-85c3-1f86970bbade.png">

- A large percentage of the exits never happened. The least used exit type is listed below along with the remaining ones:

<img width="337" alt="166074499-55c5e531-261c-4ab1-abd5-6cb905be65c5" src="https://user-images.githubusercontent.com/99626312/207219707-658eb38b-7f07-4fc2-b103-f3993b9be8f4.png">

### Thank You
