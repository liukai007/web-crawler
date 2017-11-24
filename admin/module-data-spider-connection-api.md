## RabbitMq Connections连接状态汇总

+ Data
    + name(String) - 客户端名字
    + user(String) - 账户登陆名字
    + host (String) - 客户端IP
    + ssl(Boolean) - 是否加密
    + state(String) - 运行状态
    + timeout (int) - HeartBeat心跳时间
    + protocol (String) - 协议名称
    + connected_at(Long) -建立连接时间

## Connections连接状态汇总 [GET] /spider/rabbitmq/connections
+ Description

+ Parameters

+ Response 200 (application/json)
```
        {
            "data": [
                    {
                    "garbage_collection": {
                        "minor_gcs": 71,
                        "min_bin_vheap_size": 46422,
                        "min_heap_size": 233,
                        "fullsweep_after": 65535
                    },
                    "reductions": 35848,
                    "ssl_cipher": null,
                    "connected_at": 1510898081568,
                    "type": "network",
                    "ssl": false,
                    "timeout": 60,
                    "frame_max": 131072,
                    "protocol": "AMQP 0-9-1",
                    "send_oct_details": {
                        "rate": 40
                    },
                    "client_properties": {
                        "connection_name": "rabbitConnectionFactory#0",
                        "product": "RabbitMQ",
                        "copyright": "Copyright (c) 2007-2016 Pivotal Software, Inc.",
                        "capabilities": {
                            "exchange_exchange_bindings": true,
                            "connection.blocked": true,
                            "authentication_failure_close": true,
                            "basic.nack": true,
                            "publisher_confirms": true,
                            "consumer_cancel_notify": true
                        },
                        "information": "Licensed under the MPL. See http://www.rabbitmq.com/",
                        "version": "4.0.2",
                        "platform": "Java"
                    },
                    "send_pend": 0,
                    "host": "192.168.1.138",
                    "auth_mechanism": "PLAIN",
                    "state": "running",
                    "recv_oct_details": {
                        "rate": 44
                    },
                    "ssl_protocol": null,
                    "ssl_key_exchange": null,
                    "peer_cert_subject": null,
                    "peer_cert_validity": null,
                    "recv_cnt": 178,
                    "peer_port": 58275,
                    "ssl_hash": null,
                    "peer_cert_issuer": null,
                    "send_cnt": 178,
                    "recv_oct": 7367,
                    "vhost": "/",
                    "node": "rabbit@dev",
                    "channel_max": 0,
                    "channels": 2,
                    "peer_host": "192.168.1.130",
                    "port": 5672,
                    "send_oct": 6050,
                    "name": "192.168.1.130:58275 -> 192.168.1.138:5672",
                    "user": "rabbitmq",
                    "reductions_details": {
                        "rate": 218.4
                    }
                },
                {
                    "garbage_collection": {
                        "minor_gcs": 377,
                        "min_bin_vheap_size": 46422,
                        "min_heap_size": 233,
                        "fullsweep_after": 65535
                    },
                    "reductions": 15039405,
                    "ssl_cipher": null,
                    "connected_at": 1510822344221,
                    "type": "network",
                    "ssl": false,
                    "timeout": 60,
                    "frame_max": 131072,
                    "protocol": "AMQP 0-9-1",
                    "send_oct_details": {
                        "rate": 40
                    },
                    "client_properties": {
                        "connection_name": "rabbitConnectionFactory#0",
                        "product": "RabbitMQ",
                        "copyright": "Copyright (c) 2007-2016 Pivotal Software, Inc.",
                        "capabilities": {
                            "exchange_exchange_bindings": true,
                            "connection.blocked": true,
                            "authentication_failure_close": true,
                            "basic.nack": true,
                            "publisher_confirms": true,
                            "consumer_cancel_notify": true
                        },
                        "information": "Licensed under the MPL. See http://www.rabbitmq.com/",
                        "version": "4.0.2",
                        "platform": "Java"
                    },
                    "send_pend": 0,
                    "host": "192.168.1.138",
                    "auth_mechanism": "PLAIN",
                    "state": "running",
                    "recv_oct_details": {
                        "rate": 44
                    },
                    "ssl_protocol": null,
                    "ssl_key_exchange": null,
                    "peer_cert_subject": null,
                    "peer_cert_validity": null,
                    "recv_cnt": 75413,
                    "peer_port": 55984,
                    "ssl_hash": null,
                    "peer_cert_issuer": null,
                    "send_cnt": 75412,
                    "recv_oct": 2765947,
                    "vhost": "/",
                    "node": "rabbit@dev",
                    "channel_max": 0,
                    "channels": 2,
                    "peer_host": "192.168.1.181",
                    "port": 5672,
                    "send_oct": 2513850,
                    "name": "192.168.1.181:55984 -> 192.168.1.138:5672",
                    "user": "rabbitmq",
                    "reductions_details": {
                        "rate": 217.6
                    }
                }
            ]
        }
```            