﻿# Ansible_Code
 ---
- name: install and configure tomcat 9
  hosts: appserver
  become: yes
  vars:
    java_package: openjdk-11-jdk
    tomcat_username: tomcat
    tomcat_home: /opt/tomcat
    tomcat_shell: /bin/false
    tomcat_version: '9.0.78'
  tasks:
    - name: update packages and install java
      apt:
        name: "{{ java_package }}"
        update_cache: yes
        state: present
    - name: add a tomcat user
      ansible.builtin.user:
        name: "{{ tomcat_username }}"
        home: "{{ tomcat_home }}"
        shell: "{{ tomcat_shell }}"
        state: present
    - name: download the tomcat distributor
      ansible.builtin.get_url:
        url: "https://archive.apache.org/dist/tomcat/tomcat-9/v{{ tomcat_version }}/bin/apache-tomcat-{{ tomcat_version }}.tar.gz"
        dest: "/tmp/apache-tomcat-{{ tomcat_version }}.tar.gz"
    - name: untar the tomcat distribution
      ansible.builtin.unarchive:
        src: "/tmp/apache-tomcat-{{ tomcat_version }}.tar.gz"
        dest: "{{ tomcat_home }}"
        remote_src: "true"
