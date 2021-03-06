## Initialize the NimBLE host

The Nimble host is initialized via a call to `ble_hs_init()`.  This function is declared as follows:

```c
int ble_hs_init(uint8_t prio, struct ble_hs_cfg *cfg)
```

The parameters are documented below.

| **Parameter** | **Description** |
| ------------- | --------------- |
| *prio*        | The priority of the Nimble host task.  A lower number corresponds to higher priority. |
| *cfg*         |  A pointer to the desired host configuration, or *NULL* if you want to use the default settings. |

**prio**:

Unlike the controller, the host does not have any strict timing requirements.
This number should be greater than the priority of any time-critical tasks in
your application (remember, bigger number = lower priority!).  There are no
restrictions with regards to the host task's priority relative to its client
tasks.  In the below example, the host is assigned a priority of 1.


**cfg**:

As mentioned above, passing a *cfg* value of *NULL* will initialize the Nimble
host with the default configuration.  This is convenient while familiarizing
yourself with the Nimble stack, but ultimately you will probably want to use a
custom configuration.  For more information on configuring the host, see the
Nimble Configuration Guide (TBD).

Continuing with our running example, we now add Nimble host initialization to the *main()* function.  This application uses the default host configuration, so it specifies *NULL* as the second argument to `ble_hs_init()`.

```c hl_lines="1 44 45 46"
#include "host/ble_hs.h"

int
main(void)
{
	int rc;
    struct ble_hs_cfg cfg;

    /* Initialize OS */
    os_init();

    /* Set cputime to count at 1 usec increments */
    rc = cputime_init(1000000);
    assert(rc == 0);

    /* Create memory pool for Nimble packets and register with Msys */
    rc = os_mempool_init(&g_mbuf_mempool, MBUF_NUM_MBUFS,
            MBUF_MEMBLOCK_SIZE, &g_mbuf_buffer[0], "mbuf_pool");
    assert(rc == 0);

    rc = os_mbuf_pool_init(&g_mbuf_pool, &g_mbuf_mempool, MBUF_MEMBLOCK_SIZE,
                           MBUF_NUM_MBUFS);
    assert(rc == 0);

    rc = os_msys_register(&g_mbuf_pool);
    assert(rc == 0);

    /* Initialize our device address */
    g_dev_addr[0] = 0x33;
    g_dev_addr[1] = 0x22;
    g_dev_addr[2] = 0x11;
    g_dev_addr[3] = 0xcc;
    g_dev_addr[4] = 0xbb;
    g_dev_addr[5] = 0x0a;

	/* Initialize the statistics package */
    rc = stats_module_init();
    assert(rc == 0);

    /* Initialize the BLE LL */
    rc = ble_ll_init(0, 7, 260);
    assert(rc == 0);

    /* Initialize the BLE host. */
    rc = ble_hs_init(BLE_HS_TASK_PRI, NULL);
    assert(rc == 0);

    /* Start the OS */
    os_start();

    /* os start should never return. If it does, this should be an error */
    assert(0);
}
```

<br>

