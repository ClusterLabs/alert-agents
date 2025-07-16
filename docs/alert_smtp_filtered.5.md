alert_smtp_filtered.sh 5 "July 2025" alert_smtp_filtered.sh "User Manual"
==================================================

# NAME
alert_smtp_filtered.sh - Sample SMTP alert agent for pacemaker with filtering

# SYNOPSIS
pcs alert create id=filtered-smtp path=/var/lib/pacemaker/**alert_smtp_filtered.sh** options **email_sender=**noreply@example.com **RHA_alert_kind=**"fencing"


# DESCRIPTION
**alert_smtp_filtered.sh** is a sample alert agent which implements filtering by matching the 
value of pacemaker's CRM_alert_kind variable that is set when an alert is generated. This 
agent was built for a client who wished to send receive alerts whenever resources are relocated,.

By default, the email client the script expects is sendmail.

# OPTIONS

**path=**_/var/lib/pacemaker/alert_smtp_filtered.sh_

This is the path to the alert agent on the nodes' filesystems.

**email_sender=**_user@example.com_

This is what the agent will use as the "FROM" field on email alerts

**RHA_alert_kind=**_"fencing,node,resource,attribute"_

This option sets the RHA_alert_kind variable in the alert_smtp_filtered alert agent, to specify the
criteria on which alerts to allow to send to the email recipient.

Note that otherwise-unspecified alert types will be sent to the recipient regardless of the filter
specification.

_fencing_

    These alerts are generated when a node is fenced, whether automatically or automatically.

_node_

    These alerts when a node is suspended, unsuspended, rebooted, joins the cluster, etc. 
    
_resource_

    These alerts are generated when a resource is started, stopped, or fails to start. 
    
_resource_

    These alerts are generated when a resource's attribute is changed. 
    
# EXAMPLES

The following example will send alert emails whenever a node is fenced and also unhandled alerts,
but not node, resource, or attribute alerts.

```
[root@nodea ~]# pcs alert create id=filtered-smtp path=/var/lib/pacemaker/alert_smtp_filtered.sh options email_sender=noreply@example.com RHA_alert_kind="fencing"
[root@nodea ~]# pcs alert recipient add smtp-alert value=student@workstation.lab.example.com 
Error: alert 'smtp-alert' does not exist
[root@nodea ~]# pcs alert recipient add filtered-smtp value=student@workstation.lab.example.com  
[root@nodea ~]# 
```

This example will send 

```
 ~]# pcs alert create id=filtered-smtp path=/var/lib/pacemaker/alert_smtp_filtered.sh options email_sender=noreply@example.com RHA_alert_kind="node,resource,attribute"
 ~]# pcs alert recipient add filtered-smtp value=student@workstation.lab.example.com  
 ~]# 
```

# HISTORY
July 2025, Originally compiled by Kimberly Lazarski (klazarsk@redhat.com)
