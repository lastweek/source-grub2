# Notes on GRUB2

I used to look into this code while I was building
LegoOS' boot part. I was trying to find out
how GRUB load the kernel and how GRUB prepare all the information.

https://www.gnu.org/software/grub/manual/grub/grub.html#Introduction

## linux v.s. linux16

An interesting thing is that there are two ways to
load an kernel image in `grub.cfg`, either
`linux vmlinuz-3.10.0` or `linux16 vmlinuz-3.10.0`.
They have different effects, but not sure what are those differences.
I remember only the linux16 one works for me,
but not remembering why either. At least on CentOS 7, it's all linux16.

The `linux16` and `initrd16` in `grub-core/loader/i386/pc/linux.c`:
```c
GRUB_MOD_INIT(linux16)
{
  cmd_linux =
    grub_register_command ("linux16", grub_cmd_linux,
			   0, N_("Load Linux."));
  cmd_initrd =
    grub_register_command ("initrd16", grub_cmd_initrd,
			   0, N_("Load initrd."));
  my_mod = mod;
}

```

The `linux` and `initrd` in `grub-core/loader/i386/linux.c`:
```c
static grub_command_t cmd_linux, cmd_initrd;

GRUB_MOD_INIT(linux)
{
  cmd_linux = grub_register_command ("linux", grub_cmd_linux,
				     0, N_("Load Linux."));
  cmd_initrd = grub_register_command ("initrd", grub_cmd_initrd,
				      0, N_("Load initrd."));
  my_mod = mod;
}
```
