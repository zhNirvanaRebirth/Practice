# �������̡�ģ�������Loader  
## Linux�Ŀ�������  
1. ����BIOS��Ӳ����Ϣ���������Ҳ��ԣ������趨ȡ�õ�һ���ɿ�����װ��  
2. ��ȡ��ִ�е�һ������װ����MBR��boot Loader��Ҳ����grub2��spfdisk�ȳ���  
3. ����boot loader���趨����Kernel��kernel�Ὺʼ���Ӳ����������������  
4. ��Ӳ�������ɹ���Kernel����������systemd���򣬲���detault.target���̿���  
* systemdִ��sysinit.target��ʼ��ϵͳ��basic.target׼������ϵͳ  
* systemd����multi-user.target�µı��������������  
* systemdִ��multi-user.target�µ�/etc/rc.d/rc.local�ļ�  
* systemdִ��multi-user.target�µ�getty.target����¼����  
* systemdִ��graphical��Ҫ�ķ���  
## BIOS��boot loader��kernel����  
> �����BIOS������ͳ��BIOS��UEFI BIOS  
> ����˵��MBR�Ƿ���ʹ��MBR��GPT�а�װboot loader�Ĳ���  
### BIOS���������Ҽ����MBR/GPT  
�ڸ��˵��Լܹ��£�Ҫ��������ϵͳ��Ҫ����ϵͳȥ����BIOS(Basic Input Output System)����ͨ��BIOS����ȥ����CMOS��Ϣ������CMOS�е��趨ֵȡ�������ĸ���Ӳ���趨���磺CPU���ܱ��豸�Ĺ�ͨʱ��������װ�õ�����˳�򣬴��̵Ĵ�С��ϵͳʱ�䣬�ܱ߻������Ƿ�����Plug and Play��PnP����弴��װ�ã����ܱ��豸��I/Oλ�ã���CPU��ͨ��IRQ�жϵȣ���ȡ����Щ��Ϣ֮��BIOS���п������Ҳ���(Power-on Self Test,POST)��Ȼ��ʼִ��Ӳ�����Եĳ�ʼ�������趨PnPװ�ã�֮������ɿ�����װ��˳��Ȼ��ͻῪʼ���п���װ�õ����϶�ȡ��  
����ϵͳ����������ڴ����У�����BIOS��ָ��������װ���Ա����ǿ��Զ�ȡ�����еĲ���ϵͳ���ĵ��������ǲ�ͬ�Ĳ���ϵͳ���ļ�ϵͳ��ʽ����ͬ��������Ǳ���Ҫ��һ���������������������ĵ���������(load)���⣬�����������������Boot Loader�ˣ�Boot Loader��װ�ڿ���װ�õĵ�һ������(sector)�У�Ҳ��������˵��MBR(Master Boot Record,��Ҫ������¼��)�������и����⣬���ĵ�����Ҫloader����ȡ�����ǲ�ͬ�Ĳ���ϵͳ��loaderҲ�ǲ�ͬ�ģ�BIOS����ô��ȡMBR�е�boot loader���أ���ʵֻҪBIOS�ܹ���⵽���̣������а취ͨ��INT 13����ͨ������ȡ�ô����еĵ�һ�������е�MBR  
### Boot Loader�Ĺ���  
> Loader����Ҫ�Ĺ�������ʶ����ϵͳ�е��ļ���ʽ�Ӷ���������ĵ��ڴ���ȥִ��  
  
#### ���ز���ϵͳ  
ÿ�ֲ���ϵͳ����Ҫʹ���Լ���loader���ܽ������ĵ����뵽�ڴ���ִ�У�����ÿ���ļ�ϵͳ(filesystem,����partition)���ᱣ��һ�鿪������(boot sector)�ṩ����ϵͳ��װboot loader����ͨ������ϵͳԤ�趼�ᰲװһ��loader�����ĸ�Ŀ¼���ڵ��ļ�ϵͳ��boot secotr�ϡ�  
����ϵͳ��MBRֻ��һ����������ôȷ��ִ���Ǹ�boot sector�е�loader�أ������boot loader�����¹����й��ˣ�  
* �ṩѡ�����ṩʹ���߿���ѡ��Ĳ�ͬ������Ŀ  
* ��������ļ���ֱ��ָ��ɿ����ĳ�����������ʼ����ϵͳ  
* ת������loader������������ת����������loader������  
**ע�⣺�����ڰ�װϵͳ��ʱ��ϵͳ�ᱣ��һ�����������������ǰ�װMBR����boot loader�ǰ�װ��MBR�ϵģ��ڰ�װ����ϵͳ��ʱ�򣬺�װϵͳ��MBR�Ḳ��ִ��ϵͳ��MBR����ΪMBRֻ��һ��������Windows��loader�����п���Ȩת���Ĺ��ܣ������ڰ�װ��ϵͳ��ʱ����Ҫ�Ȱ���Windows���ٰ�װLinux**  
### ����������Ӳ����initramfs�Ĺ���  
> �����ļ����õ�λ�ã�/boot/�ļ����£�����ģ�����λ�ã�/lib/modules/Ŀ¼��  
  
��boot loader��ȡ�����ļ���Linux�Ὣ���Ľ�ѹ���ڴ��У�Ȼ�����ú��ĵĹ��ܣ���ʼ���Ժ��������ܱ�װ�ã��������洢װ�ã�CPU�������������ȣ�����ʱLinux���Ļ����Լ��Ĺ������²���һ��Ӳ��  
Ϊ��Ӳ�����������������Ĺ��ܿ����ߵı�����Linux�����ǿ���ͨ����̬�������ģ�飨module����ʵ����������˵������������Щ����ģ�������/lib/modules/Ŀ¼�£���Ϊ����ģ������ڴ��̸�Ŀ¼�£�������Ҫ/lib��/������һ��partition�У����ڿ��������к��ı�����ظ�Ŀ¼���������ܶ�ȡ����ģ���ṩ������������Ĺ���  
һ����˵���Ǳ���Ĺ��ܶ����Ա����Ϊģ��ĺ��Ĺ��ܣ�������һ�����������Linux��װ��SATA�������棬���ǿ���ͨ��BIOS��INT 13��ȡ��boot loader��Kernel�ļ���������Ȼ��kernel��ʼ�ӹ�ϵͳ�����Ӳ���Լ����Թ��ظ�Ŀ¼��ȡ�ö�����������򣬵��ǣ����ĸ�������ʶSATA���̣�������Ҫ����SATA���������򣬷����޷����ظ�Ŀ¼������SATA������������/lib/modules/�ڣ���û�й��ظ�Ŀ¼���������ô�ܶ�ȡ��/lib/modules/�е����������أ�����������£�������Ҫʹ�������ļ�ϵͳ�������������  
�����ļ�ϵͳ(Initial RAM Disk��Initial RAM Filesystem)һ��ʹ�õ��ļ���Ϊ/boot/initrd��/boot/initramfs����Ҳ�ܹ�ͨ��boot loader���뵽�ڴ��У�Ȼ������ļ�����ѹ�����ڴ���ģ���һ����Ŀ¼�������ģ�����ڴ��е��ļ�ϵͳ���ṩһ֧��ִ�еĳ���ͨ���ó��������뿪������������Ҫ�ĺ���ģ�飬��������ɺ󣬻�����������º���systemd����ʼ������������������  
### initramfs  
> boot loader��������kernel��initramfs��Ȼ�����ڴ�����initramfs��ѹ��Ϊ��Ŀ¼��kernel�����Դ������ʵ����������������ͷ������ļ�ϵͳ��������ʵ�ʵĸ�Ŀ¼�ļ�ϵͳ����ʼ������������������  
  
initramfs��ʵ��һ��С�͵ĸ�Ŀ¼�����С�͵ĸ�Ŀ¼Ҳ��ͨ��systemd�����й�����ͨ��initrd.target��������initrd.targetҲ��Ҫ����һ��basic.target��sysinit.target��Ӳ����⡢���Ĺ��ܿ��������̣�Ȼ��ʼ��ϵͳ˳������������ж��initramfs�ļ�ϵͳ��ʵ�ʹ���ϵͳ�ĸ�Ŀ¼  
### ��һ֧����systemd��ʹ��default.target���뿪������  
> systemd�ǵ�����������ϣ��������Ӳ��������������֮�󣬺����������еĵ�һ֧��������Ҫ������׼�����ִ�еĻ���������ϵͳ���������ƣ������趨�����Դ����ļ�ϵͳ��ʽ����������ȣ����еĶ�����ͨ��/etc/systemd/system/default.target�趨����������  
  
#### �����Ĳ�������target��������runlevel�ĵȼ�  
> Ԥ��Ĳ�������(default.target)����multi-user.target��graphical.target,�Լ�rescure.target, emergency.target, shutdown.target, initrd.target��  
  
systemdΪ��������systemV������Ϊ����runlevel������������˽�ϣ�ʹ��ָ�"ls -lid /usr/lib/systemd/system/runlevel*.target | cut -c 28-"�ɲ鿴runlevel�Ͳ��������Ķ�Ӧ��ϵ  
systemd�Ĵ������̣�  
1. ���ӵ�/usr/lib/systemd/system/Ŀ¼ȥȡ��multi-user.target��graphical.target������/usr/lib/systemd/system/graphical.target��/usr/lib/systemd/system/multi-user.target�е��趨���������Ŀ������  
2. �ҵ�/etc/systemd/system/graphical.target.wants/��/etc/systemd/system/multi-user.target.wants/Ŀ¼�µ�ʹ�����趨�����unit  
3. �ҵ�/usr/lib/systemd/system/graphical.target.wants/��/usr/lib/systemd/system/multi-user.target.wants/Ŀ¼��ϵͳ�趨�����unit  
### systemdִ��sysinit.target��ʼ��ϵͳ��basic.target׼��ϵͳ  
sysinit.targetִ�еĲ�����  
* �����ļ�ϵͳװ�õĹ��أ�����dev-hugepages.mount, dev-mqueue.mount�ȹ��ط��񣬹��سɹ������/dev/Ŀ¼�½�����Ӧ��Ŀ¼  
* �����ļ�ϵͳ�����ã������������У��������(iscsi)��LVM�ļ�ϵͳ���ļ�ϵͳ���շ���(multipath)��  
* �������̵���Ϣ�����붯��ִ�У�ʹ��plymouthd�������playmouthָ�������ݶ�������Ϣ  
* ��־ʽ��¼����ʹ�ã���������systemd-journald����  
* �������ĺ���ģ�飺ͨ��/etc/modules-load.d/*.conf�ļ����趨  
* �������ĺ��Ĳ����趨������/etc/sysctl.conf�Լ�/etc/sysctl.d/*.conf�ڲ����趨  
* ����ϵͳ���������������������������԰���ϵͳ����һЩ�����������Ĺ���  
* �趨�ն˻�(console)����  
* ������̬װ�ù���Ա������udevd�����ڶ�̬��Ӧʵ��װ�ô�Ǯ��װ���ļ�����Ӧ��һ������  
basic.targetִ�еĲ�����  
* ����alsa��Ч��������  
* ����firewalls����ǽ  
* ����CPU��΢ָ���  
* �������趨SELinux�İ�ȫ�ı�  
* ��Ŀǰ���������в����Ŀ�����Ϣд��/var/log/dmesg��  
* ��/etc/sysconfig/modules/*.modules��/etc/rc.modules�������Աָ����ģ��  
* ����systemd֧Ԯ��timer����  
�������׶�֮��ϵͳ�Ѿ�����˳�������У��Ͳ��¼����������񣬱�����֤�����service��������  
### systemd����multi-user.target�µķ���  
> ��������������󣬾���sysinit.target��ʼ��������ϵͳ���Դ�ȡ֮��basic.target��ϵͳ��Ϊ����ϵͳ�Ļ���֮�������÷�����˳�����У�����Ҫ�����������������Լ��ṩ���������ܵ����������Щ�����������multi-user.target�����£��ɲ鿴/etc/systemd/system/multi-user.target.wants/��������ݣ�  
  
### ���������л�ʹ�õ���Ҫ�趨��  
* /etc/modules-load.d/*.conf:Ҫ��������ģ���λ��  
* /etc/modprobe.d/*.conf:���Լ���ģ�������λ��  
* /etc/sysconfig/*:�����Ļ����趨��  
## ���������ģ��  
> �ڿ��������У��ܷ�ɹ�����������������Ӳ���豸���Ǻ���(kernel)�Ĺ���������һ����ѹ���ļ���ʹ�ú���֮ǰ����Ҫ�����ѹ֮�󣬲������뵽�ڴ��У�Ŀǰ�ĺ��Ķ����ж�ȡģ�黯��������Ĺ��ܣ�����modules(ģ�黯)�Ĺ���  
  
���ĺͺ���ģ������ļ���  
* ���ģ�/boot/vmlinuz��/boot/vmlinuz-version  
* ���Ľ�ѹ����RAM Disk��/boot/initramfs��/boot/initramfs-version��  
* ����ģ�飺/lib/modules/version/kernel��/lib/modules/$(uname -r)/kernel  
* ����ԭʼ��:/usr/src/linux��/usr/src/kernels/  
* ���İ汾��/proc/version  
* ϵͳ���Ĺ��ܣ�/proc/sys/kernel/  
### �������и��µ�Ӳ��������ϵͳ��֧�֣�������Ҫ���Ĳ���  
1. ���±�����ģ��������µ�Ӳ����������Դ��  
2. ��Ӳ����������������ģ�飬���ڿ���ʱ�����ģ��  
### ����ģ��Ĺ۲�  
* ʹ��ָ��"lsmod"�鿴Ŀǰ���������˶���ģ��  
* ʹ��ָ��"modinfo [-F field] [-adln] [module_name|filename]"�鿴ĳ��ģ�����Ϣ  
* ʹ��ָ��"insmod [/full/path/module_name] [parameters]"������ָ����ģ��  
* ʹ��ָ��"rmmod [-fw] module_name"��ж��ָ����ģ��  
* ʹ��ָ��"modprobe [-cfr] module_name"�����ػ�ж��ָ����ģ�飨modprobe�����insmod/rmmod�ĺô�����modprobe��������װ/ж��ģ���������(����modules.dep�е�����)���Ҳ���Ҫ����·����  
## Boot Loader��Grub2  
> boot loader��������ĵ���Ҫ���ߣ�û��boot loader��kernel����û�а취��ϵͳ����  
  
boot loader��װ��MBR�У�MBR���������̵ĵ�һ��sector�ڵ�һ�����飬��Сֻ��446bytes���������ǵ�boot loader�ĳ�������趨���ϲ�����ȫ��������MBR�У�����Linux��ִ��boot loader�ĳ�������趨ֵ������Ϊ�����׶�(stage)  

### boot loader�������׶�(stage)  
1. ִ��boot loader�����������������뱻��װ�ڿ�����(MBR)  
2. �����������趨����ͨ��boot loader�������е��趨������صĻ��������ļ�(��/boot�£�����grub2�йص��趨��������/boot/grub2/Ŀ¼��)  
### grub2���趨����/boot/grub2/grub.cfg  
grub2���ŵ㣺  
* ��ʶ��֧Ԯ�϶���ļ�ϵͳ��������ʹ��grub2��������ֱ�����ļ�ϵͳ�����������ļ���  
* ������ʱ�򣬿������б������޸Ŀ����趨��Ŀ������bash��ָ��ģʽ  
* ���Զ�̬�������趨����������Ҫ���޸��趨�������°�װgrub2��Ҳ����ֻҪ�޸�/boot/grub2/grub.cfg�е����ݣ��´ο�������Ч��  
