check_recursive_validation
==========================
Nagios plugin to check validation status of a recursive name server.

The plugin sends one or more queries (./DNSKEY by default) to a recursive name server.  If the first response comes back with AD=1, the plugin reports SUCCESS.

If the first response comes back as SERVFAIL, the plugin sends another query with CD=1.  If the second response comes back, the plugin assumes the first error was due to a DNSSEC validation problem and it reports a CRITICAL error.  However, if the second response also comes back as SERVFAIL, the plugin returns a WARNING for general resolution failure.

Timeouts generate a WARNING.

All other responses generate a CRITICAL error.

requirements
------------
* Perl
* Net::DNS module


usage
-----

    define command {
     command_name    check-recursive-validation
     command_line    /usr/local/libexec/nagios-local/check_recursive_validation -H $HOSTADDRESS$
    }
     
    define service {
     name                dns-recursive-validation
     check_command       check-recursive-validation
     register            0
     ...
    }
     
    define service {
     use dns-recursive-validation
     host_name 127.0.0.1
    }
