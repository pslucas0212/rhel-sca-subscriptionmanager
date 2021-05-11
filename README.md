# RHEL 8 using Subscription Manager with SCA Enabled

- Enable SCA on your Red Hat customer portal account - How do I enable Simple Content Access for Red Hat Subscription Management? - in the [Simple Content Access article](https://access.redhat.com/articles/simple-content-access)

- Create an activation key with no subscriptions attached. 

- Register a system via subscription manager for RHEL 8.0+, 7.0+ and 6.0+

      # subscription-manager register --org=xxxxxxxxx --activationkey=rhel_test 
      
- Verify that the registered system content access mode is set to SCA.  Note: You will only see the SCA status on RHEL 7.0+ and RHEL 8.0+

      # subscription-manager status
      
- Check which repos are enabled

      # subscription-manager repos --list-enabled
     
- Repo enablement example

      # subscription-manager repos --enable rhel-6-server-els-rpms    
      
- You can still attach specific subs (ELS subscription) if you want to track their subscription usage.  Find the Pool ID of the subcription you want to attach, and then subscribe to that pool.

      # subscription-manager list --available
      # subscription-manager subscribe --pool=xxxxxxxxxxxxxxxxxxxxxxxx
      
- Insights setup for RHEL 8

      # insights-client --enable
      
- Insights setup for RHEL 7.0+ and 6.4+

      # yum -y install insights-client
      # insights-client --enable
      
      


## Reference

- [Subscription Manager Command Cheat Sheet](https://access.redhat.com/sites/default/files/attachments/rh_sm_command_cheatsheet_1214_jcs_print.pdf)
