iransam3.json
        "detour": "direct"
      },
      {
        "tag": "dns-local",
        "address": "local",
        "detour": "direct"
      },
      {
        "tag": "dns-block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "domain": "cp.cloudflare.com",
        "server": "dns-remote",
        "rewrite_ttl": 3000
      }
    ],
    "final": "dns-remote",
    "static_ips": {
      "sky.rethinkdns.com": [
        "188.114.99.0",
        "188.114.98.0",
        "2a06:98c1:3122::",
        "2a06:98c1:3123::",
        "104.17.148.22",
        "104.17.147.22",
        "188.114.96.6",
        "188.114.97.6",
        "2a06:98c1:3121::6",
        "2a06:98c1:3120::6"
      ]
    },
    "independent_cache": true
  },
  "inbounds": [
    {
      "type": "tun",
      "tag": "tun-in",
      "mtu": 9000,
      "inet4_address": "172.19.0.1/28",
      "auto_route": true,
      "endpoint_independent_nat": true,
      "stack": "mixed",
      "sniff": true,
      "sniff_override_destination": true
    },
    {
      "type": "mixed",
      "tag": "mixed-in",
      "listen": "127.0.0.1",
      "listen_port": 2334,
      "sniff": true,
      "sniff_override_destination": true
    },
    {
      "type": "direct",
      "tag": "dns-in",
      "listen": "127.0.0.1",
      "listen_port": 6450
    }
  ],
  "outbounds": [
    {
      "type": "selector",
      "tag": "select",
      "outbounds": [
        "auto",
        "ایران🇮🇷 § 0",
        "آلمان🇩🇪WoW § 1"
      ],
      "default": "auto"
    },
    {
      "type": "urltest",
      "tag": "auto",
      "outbounds": [
        "ایران🇮🇷 § 0",
        "آلمان🇩🇪WoW § 1"
      ],
      "url": "http://t.me/",
      "interval": "10m0s",
      "idle_timeout": "1h40m0s"
    },
    {
      "type": "wireguard",
      "tag": "ایران🇮🇷 § 0",
      "local_address": [
        "172.16.0.2/24",
        "2606:4700:110:8d29:424c:4e6b:746b:3e67/128"
      ],
      "private_key": "aG398qqwx8OGMd37IwTa5eXVHi/9gE8JULrE78nZ7UE=",
      "server": "162.159.195.130",
      "server_port": 1387,
      "peer_public_key": "bmXOC+F1FxEMF9dyiK2H5/1SUtzH0JuVo51h2wPfgyo=",
      "reserved": "g0yC",
      "mtu": 1330,
      "fake_packets": "10-20",
      "fake_packets_size": "20-60",
      "fake_packets_delay": "5-10"
    },
    {
      "type": "wireguard",
      "tag": "آلمان🇩🇪WoW § 1",
      "detour": "ایران🇮🇷 § 0",
      "local_address": [
        "172.16.0.2/24",
        "2606:4700:110:86f4:8ed0:be1f:4174:9874/128"
      ],
      "private_key": "cF1c/2mMZeSKNPTup09zzZFRseuXbBd08Qwoh8wlbHk=",
      "server": "162.159.192.132",
      "server_port": 1010,
      "peer_public_key": "bmXOC+F1FxEMF9dyiK2H5/1SUtzH0JuVo51h2wPfgyo=",
      "reserved": "JZpB",
      "mtu": 1280
    },
    {
      "type": "dns",
      "tag": "dns-out"
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "direct",
      "tag": "direct-fragment",
      "tls_fragment": {
        "enabled": true,
        "size": "5-10",
        "sleep": "1-5"
      }
    },
    {
      "type": "direct",
      "tag": "bypass"
    },
    {
      "type": "block",
      "tag": "block"
    }
  ],
  "route": {
    "geoip": {
      "path": "geo-assets/sagernet-sing-geoip-geoip.db"
    },
    "geosite": {
      "path": "geo-assets/sagernet-sing-geosite-geosite.db"
    },
    "rules": [
      {
        "inbound": "dns-in",
        "outbound": "dns-out"
      },
      {
        "port": 53,
        "outbound": "dns-out"
      },
      {
        "clash_mode": "Direct",
        "outbound": "direct"
      },
      {
        "clash_mode": "Global",
        "outbound": "select"
      }
    ],
    "final": "select",
    "auto_detect_interface": true,
    "override_android_vpn": true
  },
  "experimental": {
    "cache_file": {
      "enabled": true,
      "path": "clash.db"
    },
    "clash_api": {
      "external_controller": "127.0.0.1:6756",
      "secret": "tgLTFRg8D8sIGsCt"
    }
  }
}