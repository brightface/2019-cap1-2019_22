�ҽ� �ڵ带 �����ϴ� ���� 
�ҽ� �ڵ� ���� ��, �ʿ��� ������ �Ϲ������� lib ������ �����Ͽ� �����Ѵ�.

src ���� ������ �Ʒ� ���� �Ͽ��� �����Ӱ� �����Ѵ�. 
	-������ �ҽ� �ڵ带 ���� ������ �ߺ� �������� �ʴ´�. 
	-�ҽ� �ڵ� �������� ������ �������� �ʴ´�. 
	-���� �ۼ��� �ҽ� �ڵ常�� �����ǵ��� �Ѵ�. 

1. �ý��� ����
�ý��� ��ġ ���� 
����: ubuntu 14.04.02 LTS

sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git
sudo apt-get install gcc make linux-headers-$(uname -r) git-core
CSITOOL_KERNEL_TAG=csitool-$(uname -r | cut -d . -f 1-2)
git clone https://github.com/dhalperi/linux-80211n-csitool.git
cd linux-80211n-csitool
git checkout ${CSITOOL_KERNEL_TAG}
cd ..
git clone https://github.com/dhalperi/linux-80211n-csitool-supplementary.git
sudo apt-get install libpcap-dev
git clone https://github.com/dhalperi/lorcon-old.git
cd linux-80211n-csitool
make -C /lib/modules/$(uname -r)/build M=$(pwd)/drivers/net/wireless/iwlwifi modules
sudo make -C /lib/modules/$(uname -r)/build M=$(pwd)/drivers/net/wireless/iwlwifi INSTALL_MOD_DIR=updates modules_install
sudo depmod
cd ..
for file in /lib/firmware/iwlwifi-5000-*.ucode; do sudo mv $file $file.orig; done
sudo cp linux-80211n-csitool-supplementary/firmware/iwlwifi-5000-2.ucode.sigcomm2010 /lib/firmware/
sudo ln -s iwlwifi-5000-2.ucode.sigcomm2010 /lib/firmware/iwlwifi-5000-2.ucode
make -C linux-80211n-csitool-supplementary/netlink
cd lorcon-old
./configure
make
sudo make install
cd linux-80211n-csitool-supplementary/injection
Make

2. Injection mode(��Ŷ�� ������ ���)
setup_injecttest2.sh�� �ִ� �������� sudo bash setup_injecttest2.sh
sudo ./ Link\ to\ random_packets (�� ��Ŷ��) (�ѹ��� ���� ��Ŷ��) 1 (������)

3. Monitor mode(��Ŷ�� �޴� ���)
setup_monitor_csitest.sh�� �ִ� �������� sudo bash setup_monitor_csitest.sh
Supplementary/netlink ���� sudo ./log_to_file (���ϸ�)