Ansible Role - My NGINX Reverse Proxy Server
============================================

An Ansible role for setting up NGINX to serve as a reverse proxy.  Settings are
fairly specific to my needs.

Requirements
------------

None.

Role Variables
--------------

- `nginx_config_file`:  Path to the main NGINX configuration file.  Defaults to
  `/etc/nginx/nginx.conf`.
- `nginx_config_dir`: Path to the NGINX configuration directory.  Defaults to
  `{{ nginx_config_file | dirname }}`.
- `nginx_ssl_dir`:  Directory used to store SSL certificates and keys.
  Defaults to `{{ nginx_config_dir }}/ssl`.  *NOTE* This role does not manage
  SSL configuration.
- `nginx_ssl_key`:  Location of an SSL key for use with the SSL-enabled
  `server` block in the NGINX configuration file.  Defaults to
  `{{ nginx_ssl_dir }}/{{ nginx_server_name }}.key`.
- `nginx_ssl_cert`:  Location of an SSL certificate for use with the
  SSL-enabled `server` block in the NGINX configuration file.  Defaults to
  `{{ nginx_ssl_dir }}/{{ nginx_server_name }}.crt`.
- `nginx_use_ssl`:  When true, redirects all plain HTTP traffic to the HTTPS
  server.  Defaults to `yes`.
- `nginx_extra_config_dir`:  Directory where extra configurations files are
  created.  Defaults to `{{ nginx_config_dir }}/conf.d`.
- `nginx_proxy_config_file`:  Configuration file with settings appropriate to
  include in `location` blocks for reverse proxies.  Specifies, among other
  things, the content of certain headers in forwarded requests.  Defaults to
  `{{ nginx_extra_config_dir }}/proxy.conf`.
- `nginx_locations_available_dir`:  Directory where configuration files
  containing `location` blocks are stored.  Defaults to
  `{{ nginx_config_dir }}/locations-available`.
- `nginx_locations_enabled_dir`:  Directory containing symlinks to each
  `location` file in `nginx_locations_available_dir` that are marked as
  `enabled` in the `nginx_locations` list (see below).  Defaults to
  `{{ nginx_config_dir }}/locations-enabled`.
- `nginx_server_name`:  Argument to the `server` directive in the main `server`
  block of the NGINX configuration file.  Defaults to `default`.
- `nginx_owner`:  User that owns the NGINX configuration directory and
  configuration files.  Defaults to `root`.
- `nginx_group`:  Group that owns the NGINX configuration directory and
  configuration files.  Defaults to `root`.
- `nginx_locations`:  A list of hashes of the form:

```yaml
- location: '^~ somepath'   # See http://nginx.org/en/docs/http/ngx_http_core_module.html#location
  name: mylocation          # Optional - will be used to name the location
                            # configuration file; in this case it would be
                            # `{{ nginx_locations_available_dir }}/mylocation.conf'
  host: 'http://mylocation' # A URI specifying where to proxy requests.
  enabled: yes              # Whether to symlink the location configuration
                            # into the locations-enabled directory.  Defaults
                            # to `yes'.
```

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
