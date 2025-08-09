# linux-admin-writeup

### OVERVIEW
----------------------------------------------------------------------------------------------

This project documents my process of setting up a RedHat Enterprise Linux 9 lab 
inside VirtualBox.  In the near future, the configuration will be [performed inside of RHEL 9.x using
KVM as the Hypervisor.  

The Goal:  Build a home (enterprise-grade) Linux environment to develop and 
demonstrate system administration skills, gain troubleshooting experience, and 
document real-world tasks for my Linux career portfolio.

----------------------------------------------------------------------------------------------

###################            Current Milestones            ###################

#### 1)    Installation & System Setup
            +  Downloaded the official DVD.iso for RHEL 9 (x86_64 architecture)
            +  Installed RHEL 9 using **manual mode** (not unattended)
            +  Created a separate non-root user with 'sudo' privileges
            +  Configured disk, time-zone, and network manually during installation 

#### 2)    Subscription Management & Repositories
            +  Registered the system with Red Hat Subscription Manager
            +  Encountered issue due to Simple Content Access (SCA) being enabled
            +  Enabled the following repos via Red Hat Customer Portal UI:
                   a. rhel-9-for-x86_64-baseos-rpms
                   b. rhel-9-for-x86_64-appstream-rpms
                   c. rhel-9-for-x86_64-highavailability-rpms

#### 3)    Post-Install Configuration
            +  Enabled DNF plugins & updated the system:
                $ sudo dnf install dnf-plugins-core
                $ sudo dnf update 
                $ sudo dnf install vim wget curl git net-tools bash-completion
                $ sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
                $ sudo dnf install htop neofetch nmap ncgu iftop glances

#### 4)    Troubleshooting & Lessons Learned
            +  VirtualBox tried to force an unattended install -- Needed to disable it to proceed manually
            +  The "Next" button was greyed out until I selected the ISO manually and launched without 
               automation
            +  System was registered, but 'subscription-manager' showed status: disabled
            +  Could not attach pools due to SCA being enabled
            +  CLI commands like '--enable' failed or were ambiguous due to minimal toolset
            +  Had to use the Red Hat Portal to enabled software sources directly
            +  'htop' and other tools not found in official repos require manual EPEL installation
            +  'epel-release' not found via 'dnf';  the workaround was to download the '.rpm' directly
               from Fedora's servers

**Note:**   ---   These problems were solved step-by-step through documentation review, testing fixes, and
                     understanding RHEL's enterprise content model -- all part of what makes the work.

---

## Credits & Acknowledgments

**Author & Implementation**  
- **Vern** — Designed, implemented, and documented the Linux-based enterprise network and virtual machine configurations, including service deployment, hardware planning, and ongoing optimization.

**Guidance & Technical Assistance**  
- **ChatGPT (OpenAI)** — Provided structured step-by-step guidance, configuration recommendations, and network architecture planning support throughout the project.

---

