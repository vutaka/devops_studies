# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  config.vm.define :sandbox do | sandbox |
    sandbox.vm.hostname = "sandbox"
    # sandbox.vm.network :private_network, ip: "192.168.33.10", virtualbox__intnet: "intnet"

    # ポートフォワーディングの設定
    sandbox.vm.network "forwarded_port", guest: 8080, host: 38080
    
    # プロキシ設定
    if Vagrant.has_plugin?("vagrant-proxyconf") && ENV['http_proxy'] && ENV['no_proxy']
      puts '- Proxy Setting ----------------------------------'
      puts ENV['http_proxy']
      sandbox.proxy.http     = ENV['http_proxy']
      sandbox.proxy.https    = ENV['http_proxy']
      sandbox.proxy.no_proxy = ENV['no_proxy']
      puts '--------------------------------------------------'
    end
    
    # カーネルのアップデート
    sandbox.vm.provision :ansible_local do |ansible|
      ansible.playbook = "playbook/update_kernel.yml"
      ansible.compatibility_mode = "auto"
    end

    # カーネルのアップデートを反映させるため再起動
    sandbox.vm.provision :reload

    # カーネルのアップデート後に再起動が必要だったためplaybookを切り分ける
    sandbox.vm.provision :ansible_local do |ansible|
      ansible.playbook = "playbook/setup_ci.yml"
      ansible.compatibility_mode = "auto"
    end

  end


end