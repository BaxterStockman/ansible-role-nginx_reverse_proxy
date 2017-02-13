# Ansible Role - NGINX Reverse Proxy

An Ansible role for setting up NGINX to serve as a reverse proxy.

## Requirements

This role uses the `package` module, so you should ensure that
`ansible_pkg_mgr` is defined (probably by enabling fact gathering on the play
in which this role is applied).

## Role Variables

- `nginx_config_file`:  Path to the main NGINX configuration file.  Defaults to
  `/etc/nginx/nginx.conf`.
- `nginx_config_dir`: Path to the NGINX configuration directory.  Defaults to
  `{{ nginx_config_file | dirname }}`.
- `nginx_server_name`:  Argument to the `server` directive in the main `server`
  block of the NGINX configuration file.  Can be a string or list of strings.
  Defaults to `default`.
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
- `nginx_server_config_file`:  Destination of configuration file with settings
  appropriate to include in `server` blocks. Defaults to
  `{{ nginx_extra_config_dir }}/server.conf`.
- `nginx_proxy_config_file`:  Destination of configuration file with settings
  appropriate to include in `location` blocks for reverse proxies.  Specifies,
  among other things, the content of certain headers in forwarded requests.
  Defaults to `{{ nginx_extra_config_dir }}/proxy.conf`.
- `nginx_owner`:  User that owns the NGINX configuration directory and
  configuration files.  Defaults to `root`.
- `nginx_group`:  Group that owns the NGINX configuration directory and
  configuration files.  Defaults to `root`.
- `nginx_locations_available_dir`:  Directory where configuration files
  containing `location` blocks will be placed.  Defaults to
  `{{ nginx_config_dir }}/locations-available`.
- `nginx_locations_enabled_dir`:  Directory containing symlinks to each
  `location` file in `nginx_locations_available_dir` that are marked as
  `enabled` in the `nginx_locations` list (see below).  Defaults to
  `{{ nginx_config_dir }}/locations-enabled`.
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

- `nginx_sites_available_dir`:  Directory where configuration files
  containing `server` blocks will be placed.  Defaults to
  `{{ nginx_config_dir }}/sites-available`.
- `nginx_sites_enabled_dir`:  Directory containing symlinks to each `server`
  file in `nginx_sites_available_dir` that are marked as `enabled` in the
  `nginx_sites` list (see below).  Defaults to `{{ nginx_config_dir }}/sites-enabled`.
- `nginx_sites`:  A list of hashes of the form:

```yaml
- name: mysite              # Optional - will be used to name the site
                            # configuration file; in this case it would be
                            # `{{ nginx_sites_available_dir }}/mysite.conf'
- server_name:              # See http://nginx.org/en/docs/http/server_names.html
    - foo                   # Takes a string or list of strings
    - foo.com
  enabled: yes              # Whether to symlink the location configuration
                            # into the sites-enabled directory.  Defaults
                            # to `yes'.
  includes: /path/to.conf   # Other configurations to include.  Takes a path or
                            # list of paths.
  use_ssl: yes              # See the description for `nginx_use_ssl`,
  ssl_dir: /path/to/dir     # `nginx_ssl_dir`, `nginx_ssl_key`, and
  ssl_key: /path/to.key     # `nginx_ssl_cert`.
  ssl_cert: /path/to.crt
```

- `nginx_pkg_name`: the name of the NGINX package as provided by the package
  manager of the target system.  Defaults to `nginx`.

## Dependencies

None.

## Example Playbook

## License

MIT

## Author Information

Matt Schreiber <schreibah@gmail.com>
