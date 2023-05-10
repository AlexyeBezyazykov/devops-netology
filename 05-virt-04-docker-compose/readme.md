# Домашнее задание к занятию 4. «Оркестрация группой Docker-контейнеров на примере Docker Compose»

## Задача 1

Создайте собственный образ любой операционной системы (например ubuntu-20.04) с помощью Packer ([инструкция](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/packer-quickstart)).

Чтобы получить зачёт, вам нужно предоставить скриншот страницы с созданным образом из личного кабинета YandexCloud.

    yc compute image list
    +----------------------+---------------+--------+----------------------+--------+
    |          ID          |     NAME      | FAMILY |     PRODUCT IDS      | STATUS |
    +----------------------+---------------+--------+----------------------+--------+
    | fd8qrt6pg67eqopb7db9 | centos-7-base | centos | f2ei2tsbd97v7jap5rhc | READY  |
    +----------------------+---------------+--------+----------------------+--------+

  ![Виртуальные машины | Yandex Compute Cloud 2023-05-10 15-51-46]([Виртуальные машины | Yandex Compute Cloud 2023-05-10 15-51-46.png](https://github.com/AlexyeBezyazykov/devops-netology/blob/main/05-virt-04-docker-compose/%D0%92%D0%B8%D1%80%D1%82%D1%83%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5%20%D0%BC%D0%B0%D1%88%D0%B8%D0%BD%D1%8B%20%7C%20Yandex%20Compute%20Cloud%202023-05-10%2015-51-46.png))


## Задача 2

**2.1.** Создайте вашу первую виртуальную машину в YandexCloud с помощью web-интерфейса YandexCloud.        

**2.2.*** **(Необязательное задание)**      
Создайте вашу первую виртуальную машину в YandexCloud с помощью Terraform (вместо использования веб-интерфейса YandexCloud).
Используйте Terraform-код в директории ([src/terraform](https://github.com/netology-group/virt-homeworks/tree/virt-11/05-virt-04-docker-compose/src/terraform)).

Чтобы получить зачёт, вам нужно предоставить вывод команды terraform apply и страницы свойств, созданной ВМ из личного кабинета YandexCloud.

    natalia@MacBook-Pro-Natalia terraform % terraform init

    Initializing the backend...

    Initializing provider plugins...
    - Reusing previous version of yandex-cloud/yandex from the dependency lock file
    - Using previously-installed yandex-cloud/yandex v0.88.0

    Terraform has been successfully initialized!

    You may now begin working with Terraform. Try running "terraform plan" to see
    any changes that are required for your infrastructure. All Terraform commands
    should now work.

    If you ever set or change modules or backend configuration for Terraform,
    rerun this command to reinitialize your working directory. If you forget, other
    commands will detect it and remind you to do so if necessary.
    natalia@MacBook-Pro-Natalia terraform % terraform plan

    Terraform used the selected providers to generate the following execution plan. Resource actions are
    indicated with the following symbols:
      + create

    Terraform will perform the following actions:

      # yandex_compute_instance.node01 will be created
      + resource "yandex_compute_instance" "node01" {
          + allow_stopping_for_update = true
          + created_at                = (known after apply)
          + folder_id                 = (known after apply)
          + fqdn                      = (known after apply)
          + gpu_cluster_id            = (known after apply)
          + hostname                  = "node01.netology.cloud"
          + id                        = (known after apply)
          + name                      = "node01"
          + network_acceleration_type = "standard"
          + platform_id               = "standard-v1"
          + service_account_id        = (known after apply)
          + status                    = (known after apply)
          + zone                      = "ru-central1-a"

          + boot_disk {
              + auto_delete = true
              + device_name = (known after apply)
              + disk_id     = (known after apply)
              + mode        = (known after apply)

              + initialize_params {
                  + block_size  = (known after apply)
                  + description = (known after apply)
                  + image_id    = "fd8qrt6pg67eqopb7db9"
                  + name        = "root-node01"
                  + size        = 50
                  + snapshot_id = (known after apply)
                  + type        = "network-nvme"
                }
            }

          + network_interface {
              + index              = (known after apply)
              + ip_address         = (known after apply)
              + ipv4               = true
              + ipv6               = (known after apply)
              + ipv6_address       = (known after apply)
              + mac_address        = (known after apply)
              + nat                = true
              + nat_ip_address     = (known after apply)
              + nat_ip_version     = (known after apply)
              + security_group_ids = (known after apply)
              + subnet_id          = (known after apply)
            }

          + resources {
              + core_fraction = 100
              + cores         = 8
              + memory        = 8
            }
        }

      # yandex_vpc_network.default will be created
      + resource "yandex_vpc_network" "default" {
          + created_at                = (known after apply)
          + default_security_group_id = (known after apply)
          + folder_id                 = (known after apply)
          + id                        = (known after apply)
          + labels                    = (known after apply)
          + name                      = "net"
          + subnet_ids                = (known after apply)
        }

      # yandex_vpc_subnet.default will be created
      + resource "yandex_vpc_subnet" "default" {
          + created_at     = (known after apply)
          + folder_id      = (known after apply)
          + id             = (known after apply)
          + labels         = (known after apply)
          + name           = "subnet"
          + network_id     = (known after apply)
          + v4_cidr_blocks = [
              + "192.168.101.0/24",
            ]
          + v6_cidr_blocks = (known after apply)
          + zone           = "ru-central1-a"
        }

    Plan: 3 to add, 0 to change, 0 to destroy.

    Changes to Outputs:
      + external_ip_address_node01_yandex_cloud = (known after apply)
      + internal_ip_address_node01_yandex_cloud = (known after apply)

    ──────────────────────────────────────────────────────────────────────────────────────────────────────────

    Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these
    actions if you run "terraform apply" now.
    natalia@MacBook-Pro-Natalia terraform % terraform apply

    Terraform used the selected providers to generate the following execution plan. Resource actions are
    indicated with the following symbols:
      + create

    Terraform will perform the following actions:

      # yandex_compute_instance.node01 will be created
      + resource "yandex_compute_instance" "node01" {
          + allow_stopping_for_update = true
          + created_at                = (known after apply)
          + folder_id                 = (known after apply)
          + fqdn                      = (known after apply)
          + gpu_cluster_id            = (known after apply)
          + hostname                  = "node01.netology.cloud"
          + id                        = (known after apply)
          + name                      = "node01"
          + network_acceleration_type = "standard"
          + platform_id               = "standard-v1"
          + service_account_id        = (known after apply)
          + status                    = (known after apply)
          + zone                      = "ru-central1-a"

          + boot_disk {
              + auto_delete = true
              + device_name = (known after apply)
              + disk_id     = (known after apply)
              + mode        = (known after apply)

              + initialize_params {
                  + block_size  = (known after apply)
                  + description = (known after apply)
                  + image_id    = "fd8qrt6pg67eqopb7db9"
                  + name        = "root-node01"
                  + size        = 50
                  + snapshot_id = (known after apply)
                  + type        = "network-nvme"
                }
            }

          + network_interface {
              + index              = (known after apply)
              + ip_address         = (known after apply)
              + ipv4               = true
              + ipv6               = (known after apply)
              + ipv6_address       = (known after apply)
              + mac_address        = (known after apply)
              + nat                = true
              + nat_ip_address     = (known after apply)
              + nat_ip_version     = (known after apply)
              + security_group_ids = (known after apply)
              + subnet_id          = (known after apply)
            }

          + resources {
              + core_fraction = 100
              + cores         = 8
              + memory        = 8
            }
        }

      # yandex_vpc_network.default will be created
      + resource "yandex_vpc_network" "default" {
          + created_at                = (known after apply)
          + default_security_group_id = (known after apply)
          + folder_id                 = (known after apply)
          + id                        = (known after apply)
          + labels                    = (known after apply)
          + name                      = "net"
          + subnet_ids                = (known after apply)
        }

      # yandex_vpc_subnet.default will be created
      + resource "yandex_vpc_subnet" "default" {
          + created_at     = (known after apply)
          + folder_id      = (known after apply)
          + id             = (known after apply)
          + labels         = (known after apply)
          + name           = "subnet"
          + network_id     = (known after apply)
          + v4_cidr_blocks = [
              + "192.168.101.0/24",
            ]
          + v6_cidr_blocks = (known after apply)
          + zone           = "ru-central1-a"
        }

    Plan: 3 to add, 0 to change, 0 to destroy.

    Changes to Outputs:
      + external_ip_address_node01_yandex_cloud = (known after apply)
      + internal_ip_address_node01_yandex_cloud = (known after apply)

    Do you want to perform these actions?
      Terraform will perform the actions described above.
      Only 'yes' will be accepted to approve.

      Enter a value: yes

    yandex_vpc_network.default: Creating...
    yandex_vpc_network.default: Creation complete after 4s [id=enpknm0qh3m0qqbqhdfk]
    yandex_vpc_subnet.default: Creating...
    yandex_vpc_subnet.default: Creation complete after 1s [id=e9b7dqgn8mp7im2v3v4u]
    yandex_compute_instance.node01: Creating...
    yandex_compute_instance.node01: Still creating... [10s elapsed]
    yandex_compute_instance.node01: Still creating... [20s elapsed]
    yandex_compute_instance.node01: Still creating... [30s elapsed]
    yandex_compute_instance.node01: Still creating... [40s elapsed]
    yandex_compute_instance.node01: Still creating... [50s elapsed]
    yandex_compute_instance.node01: Creation complete after 53s [id=fhm0u144g3i6g7uu7g01]

    Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

    Outputs:

    external_ip_address_node01_yandex_cloud = "51.250.7.241"
    internal_ip_address_node01_yandex_cloud = "192.168.101.19"

## Задача 3

С помощью Ansible и Docker Compose разверните на виртуальной машине из предыдущего задания систему мониторинга на основе Prometheus/Grafana.
Используйте Ansible-код в директории ([src/ansible](https://github.com/netology-group/virt-homeworks/tree/virt-11/05-virt-04-docker-compose/src/ansible)).

Чтобы получить зачёт, вам нужно предоставить вывод команды "docker ps" , все контейнеры, описанные в [docker-compose](https://github.com/netology-group/virt-homeworks/blob/virt-11/05-virt-04-docker-compose/src/ansible/stack/docker-compose.yaml),  должны быть в статусе "Up".

    $ sudo docker ps
    CONTAINER ID   IMAGE                              COMMAND                  CREATED      STATUS                PORTS                                                                              NAMES
    9a170f364a11   prom/node-exporter:v0.18.1         "/bin/node_exporter …"   4 days ago   Up 4 days             9100/tcp                                                                           nodeexporter
    b44c87d5a9ac   gcr.io/cadvisor/cadvisor:v0.47.0   "/usr/bin/cadvisor -…"   4 days ago   Up 4 days (healthy)   8080/tcp                                                                           cadvisor
    95718d52c655   grafana/grafana:7.4.2              "/run.sh"                4 days ago   Up 4 days             3000/tcp                                                                           grafana
    dfc9e9e08214   prom/alertmanager:v0.20.0          "/bin/alertmanager -…"   4 days ago   Up 4 days             9093/tcp                                                                           alertmanager
    cb04f659b0a5   prom/prometheus:v2.17.1            "/bin/prometheus --c…"   4 days ago   Up 4 days             9090/tcp                                                                           prometheus
    051234ad4082   prom/pushgateway:v1.2.0            "/bin/pushgateway"       4 days ago   Up 4 days             9091/tcp                                                                           pushgateway
    7046e79c9df8   stefanprodan/caddy                 "/sbin/tini -- caddy…"   4 days ago   Up 4 days             0.0.0.0:3000->3000/tcp, 0.0.0.0:9090-9091->9090-9091/tcp, 0.0.0.0:9093->9093/tcp   caddy

## Задача 4

1. Откройте веб-браузер, зайдите на страницу http://<внешний_ip_адрес_вашей_ВМ>:3000.
2. Используйте для авторизации логин и пароль из [.env-file](https://github.com/netology-group/virt-homeworks/blob/virt-11/05-virt-04-docker-compose/src/ansible/stack/.env).
3. Изучите доступный интерфейс, найдите в интерфейсе автоматически созданные docker-compose-панели с графиками([dashboards](https://grafana.com/docs/grafana/latest/dashboards/use-dashboards/)).
4. Подождите 5-10 минут, чтобы система мониторинга успела накопить данные.

Чтобы получить зачёт, предоставьте: 

- скриншот работающего веб-интерфейса Grafana с текущими метриками, как на примере ниже.
<p align="center">
  <img width="1200" height="600" src="./assets/yc_02.png">
</p>

![Monitor Services - Grafana 2023-05-10 15-50-52]([Monitor Services - Grafana 2023-05-10 15-50-52.png](https://github.com/AlexyeBezyazykov/devops-netology/blob/main/05-virt-04-docker-compose/Monitor%20Services%20-%20Grafana%202023-05-10%2015-50-52.png))

