# K-Ras4B
The Codes Used for Molecular Dynamics Simulations in “A Comprehensive View of K-Ras4B Transformations Regulated by Oncogenic G12D Mutation and Phosphorylation in Phase-Segregated Membranes”

python2 insane.py -f marker.pdb -pbc rectangular -x 20 -y 20 -z 13 -u DPPC:40 -u DIPC:40 -u CHOL:20 -l DPPC:39 -l DIPC:39 -l CHOL:20 -l POP6:2 -o membrane.gro -dm -5.5 -sol W

gmx trjconv -f nowater.xtc -s nowater.tpr -n nowater.ndx -o nowater1000.xtc -dt 1000

gmx distance -f nowater1000.xtc -s nowater.tpr -select 'com of group "hvr" plus com of group "lowpo4"' -n nowater.ndx -oxyz dist_hvr-lowpo4 -tu us

gmx distance -f nowater1000.xtc -s nowater.tpr -select 'com of group "D132" plus com of group "T183"' -n nowater.ndx -oall length_D132-T183 -tu us

gmx density -f nowater1000 -s nowater -n nowater.ndx -o density_hvr-po4 -d z -b 2000000 -e 20000000

gmx mindist -f nowater1000 -s nowater -n nowater.ndx -o contact_krasbb-lowpo4p4p5 -d 0.6 -b 2000000 -e 20000000

gmx rmsdist -s nowater.tpr -f nowater1000.xtc -n nowater -o distrmsd.xvg

gmx select -f nowater1000 -s nowater -n nowater.ndx -oi hvr-pip2 -select '"Near by POP6" resname POP6 and within 0.6 of group "hvr"' -tu us -seltype atom

gmx gangle -f nowater1000.xtc -s nowater.tpr -g1 vector -n nowater.ndx -group1 'com of group "5BB" plus com of group "9BB" permute 1 2' -g2 z -oall angles

gmx trjconv -f nowater1000.xtc -s nowater.tpr -n nowater.ndx -center -o nowater-hvrcen.xtc -pbc mol -b 2000000 -e 20000000

gmx densmap -f nowater-hvrcen.xtc -s nowater.tpr -n nowater.ndx -aver z -o hvr-dppc.xpm -dmax 2
gmx xpm2ps -f hvr-dppc.xpm -o hvr-dppc.eps -rainbow blue

gmx rdf -f nowater1000 -s nowater -n nowater.ndx -o rdf_cyf-dppc -bin 0.02

gmx rmsdist -s nowater.tpr -f nowater.xtc -n nowater -o distrmsd.xvg    (krasca)

gmx gangle -f nowater.xtc -s nowater.tpr -g1 vector -n nowater.ndx -group1 'com of group "K5" plus com of group "V9" permute 1 2' -g2 z -oall angles

gmx sasa -s nowater.tpr -f nowater.xtc -n nowater -o sasa_switch1

gmx rmsf -f nowater.xtc -s nowater.tpr -n nowater.ndx -oq bfac.pdb -ox xaver.pdb -o res

gmx gangle -f nowater.xtc -s nowater.tpr -n nowater.ndx -g1 vector -group1 'com of group "T87" plus com of group "K104" permute 1 2' -g2 vector -group2 'com of group "T127" plus com of group "Y137" permute 1 2' -oall  angles_a3-a4 -tu ns

gmx distance -f nowater.xtc -s nowater.tpr -select 'com of group "D132" plus com of group "T183"' -n nowater.ndx -oall length_D132-T183 -tu ns

gmx select -f nowater -s nowater -n nowater.ndx -oi hvr-pip2 -select '"Near by POPI" resname POPI and within 0.6 of group "hvr"' -tu us -seltype atom

gmx mindist -f nowater -s nowater -n nowater.ndx -on contact_a4all-lowp -d 0.6 -tu ns

gmx angle -f nowater.xtc -n nowater.ndx -type angle -ov angle_b2-b3  (D38_D47_D57)

gmx hbond -f protein.xtc -s mdrerun.tpr -n rerun.ndx -num -hbn -hbm

