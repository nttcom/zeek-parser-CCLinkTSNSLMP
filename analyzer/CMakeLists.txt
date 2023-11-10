spicy_add_analyzer(
    NAME cc_link_tsn_slmp
    PACKAGE_NAME zeek-parser-CCLinkTSNSLMP
    SOURCES cc_link_tsn_slmp.spicy cc_link_tsn_slmp.evt zeek_cc_link_tsn_slmp.spicy
    SCRIPTS __load__.zeek main.zeek dpd.sig
)
