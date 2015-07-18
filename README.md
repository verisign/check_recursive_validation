check_recursive_validation
==========================
Nagios plugin to check validation status of a recursive name server.

The plugin sends two queries (./DNSKEY by default) to a recursive name server.  The first query is sent with DO=0 (validation disabled).  If this query fails, the plugin returns a WARNING.

The second query is sent with DO=1 (validation enabled).  If this query fails, or if the response does not have AD=1, the plugin returns a CRITICAL error.

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
