<template>
  <div id="app" style="padding: 0px;">
    <Graph />
  </div>
  <div class="overlay" id="chemViewer3D" style="width:600px;height:400px"data-widget="Kekule.ChemWidget.Viewer3D"></div>
</template>

<script>
import Graph from './components/Graph.vue';
import * as THREE from 'three';
import { Kekule } from 'kekule';
Kekule.externalResourceManager.register('three.js', THREE);

// define([ "path/to/raphael" ], function(Raphael) {
//   Kekule.externalResourceManager.register('Raphael.js', Raphael);
// });
export default {
  name: 'App',
  components: {
    Graph
  }
};
var mol = new Kekule.Molecule();
mol.appendNode(new Kekule.Atom().setSymbol('C').setCoord2D({x: 0, y: 0.80}));
mol.appendNode(new Kekule.Atom().setSymbol('C').setMassNumber(13).setCoord2D({x: -0.69, y: 0.40}));
mol.appendNode(new Kekule.Atom().setSymbol('C').setCoord2D({x: -0.69, y: -0.40}));
mol.appendNode(new Kekule.Pseudoatom().setAtomType(Kekule.PseudoatomType.ANY).setCoord2D({x: 0, y: -0.80}));
mol.appendNode(new Kekule.Atom().setSymbol('C').setCoord2D({x: 0.69, y: -0.40}));
mol.appendNode(new Kekule.Atom().setSymbol('C').setCoord2D({x: 0.69, y: 0.40}));
mol.appendNode(new Kekule.VariableAtom().setAllowedIsotopeIds(['F', 'Cl', 'Br']).setCoord2D({x: 1.39, y: 0.80}));
mol.appendBond([0, 1], 1);
mol.appendBond([1, 2], 2);
mol.appendBond([2, 3], 1);
mol.appendBond([3, 4], 2);
mol.appendBond([4, 5], 1);
mol.appendBond([5, 0], 2);
mol.appendBond([5, 6], 1);
var chemViewer3D = Kekule.Widget.getWidgetById('chemViewer3D');
//chemViewer3D.setChemObj(mol);
</script>
<style>
body {
    margin: 0;
    height: 100vh;
}
.overlay {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1000; 
}
</style>