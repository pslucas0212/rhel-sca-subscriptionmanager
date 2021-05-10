# RHEL 8 using Subscription Manager with SCA Enabled

- Enable SCA on your Red Hat customer portal account - How do I enable Simple Content Access for Red Hat Subscription Management? - in the [Simple Content Access article](https://access.redhat.com/articles/simple-content-access)

- Create an activation key with no subscriptions attached. 

- Register a system via subscription manager for RHEL 8.0+, 7.0+ and 6.?+

      # subscription-manager register --org=xxxxxxxxx --activationkey=rhel_test 
      
- Verify that the registered system content access mode is set to SCA

      # subscription-manager status
      
- Check which repos are enabled

      # subscription-manager repos --list-enabled
     
- Repo enablement example

      # subscription-manager repos --enable rhel-6-server-els-rpms    
      
- Insights setup for RHEL 8

      # insights-client --enable
      
- Insights setup for RHEL 7.0+ and 6.4+

      # yum -y install insights-client
      # insights-client --enable
