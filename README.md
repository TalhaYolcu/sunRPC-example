# RPC - sunRPC example

This repository is an example of sunRPC implentation in Ubuntu environment.

RPC yi ubuntuda kurmak için : 

```bash
sudo apt-get install rpcbind
sudo apt-get install libntirpc-dev
sudo apt-get install libtirpc-dev
```

örnek add.x kodu : 

```c
struct numbers {
    int a;
    int b;
};

program ADD_PROG {
    version ADD_VERS {
        int add(numbers)=1;
    }=1;
} = 0x23451111;
```

rpc generate kodu : 

```bash
rpcgen -a -C add.x
```

derlemek için : 

```bash
make -f Makefile.add
```

Eğer makefile hata veriyorsa CFLAGS ve LDLIBS i düzenleyin: 

```makefile
CFLAGS += -g -I/usr/include/tirpc
LDLIBS += -lnsl -ltirpc
```

server çalıştırmak için : 

```bash
./add_server
```

client çalıştırmak için

```bash
sudo ./add_clinet localhost 10 20
```

## Kodlarda yapılan değişiklikler

add_server.c içinde add_1_svc fonksiyonu içinde result için gerekli değişiklikler yapılır.

```c
printf("add(%d %d) is called\n",argp->a,argp->b);
result = argp->a + argp->b;
```

add_client.c içinde : mainde argümanlar düzenlenir, add_prog_1 parametreleri düzenlenir.

```c
add_prog_1 (host,atoi(argv[2]),atoi(argv[3]));
```

add_prog_1 fonksiyonu içi :  add_1_arg a gerekli değişkenlerin ataması yapılır.

sonuç bastırılır