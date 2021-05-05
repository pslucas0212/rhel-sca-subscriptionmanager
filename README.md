# RHEL 8 using Subscription Manager with SCA Enabled

- Enable SCA on your Red Hat customer portal account - How do I enable Simple Content Access for Red Hat Subscription Management? - in the [Simple Content Access article](https://access.redhat.com/articles/simple-content-access)

- Register a system via subscription manager

      # subscription-manager register --org=xxxxxxxxx --activationkey=rhel_test 
      
- Check which repos are enabled

      # subscription-manager repos --list-enabled
      
- Subscription manager auto-attach

      # subscription-manager auto-attach --show
      # subscription-manager autp-attache --enable/disable
      
- Insights setup for RHEL 8

      # insights-client --enable
      
- Insights setup for RHEL 7

      # yum -y install insights-client
      # insights-client --enable
